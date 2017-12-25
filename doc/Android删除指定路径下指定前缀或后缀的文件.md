
#<span  style="color:#1e819e;">Android删除指定路径下指定前缀或后缀的文件</span>

>微信公众号：CodingAndroid  
CSDN：[http://blog.csdn.net/xinpengfei521](http://blog.csdn.net/xinpengfei521)
声明：本文由CodingAndroid原创，未经授权，不可随意转载！

##需求

	我们在开发中都会遇到这样的一个需求：删除指定目录下指定的前缀或者后缀文件名的文件。

##实现思路

	对外暴露三个参数，参数一：要删除的文件目录的路径，参数二：区分是前缀还是后缀，参数三：具体前缀或者后缀字符规则。

	先枚举出路径目录下的所有文件，枚举的同时实现一个FilenameFilter接口的类，可以自定义规则，比说前缀、后缀或者其他规则，枚举的同时将我们的过滤器作为参数，这样我们就可以匹配到指定条件的文件，然后删除即可。

##实现一个删除逻辑的Runnable

代码实现的逻辑也比较简单，大家都能看得懂，下面我就直接贴代码了

```
package com.xpf.deletedemo;


import android.support.annotation.NonNull;
import android.text.TextUtils;
import android.util.Log;

import java.io.File;
import java.io.FilenameFilter;

/**
 * Created by xpf on 2017/12/25 :)
 * Function:删除指定规则的文件
 */

public class DeleteRunnable implements Runnable {

    private String mRegEx;
    private String dirPath;
    private boolean isPrefix;
    private static final String TAG = DeleteRunnable.class.getSimpleName();

    /**
     * Constructor
     *
     * @param dirPath  要删除文件所在的目录路径
     * @param isPrefix true为前缀 false为后缀
     * @param mRegEx   规则
     */
    public DeleteRunnable(String dirPath, boolean isPrefix, String mRegEx) {
        this.mRegEx = mRegEx;
        this.dirPath = dirPath;
        this.isPrefix = isPrefix;
    }

    @Override
    public void run() {
        enumAllFileList();
    }

    /**
     * 枚举并删除所有符合条件(前缀)的文件
     */
    private void enumAllFileList() {
        if (!TextUtils.isEmpty(dirPath)) {
            File adDir = new File(dirPath);
            if (adDir.exists() && adDir.isDirectory()) {
                if (!TextUtils.isEmpty(mRegEx)) {
                    DeleteFileFilter filter = new DeleteFileFilter(isPrefix, mRegEx);
                    // 2.匹配是否是需要删除的文件
                    File[] fileList = adDir.listFiles(filter);
                    if (fileList != null && fileList.length > 0) {
                        for (File file : fileList) {
                            if (file.isFile() && file.exists()) {
                                boolean delete = file.delete();
                                Log.i(TAG, "删除符合条件前缀的旧广告素材" + (delete ? "成功~" : "失败！"));
                            }
                        }
                    }
                }
            }
        }
    }

    /**
     * Created by xpf on 2017/12/25 :)
     * Function:以xxx开头或后缀的文件名的过滤器
     */
    class DeleteFileFilter implements FilenameFilter {

        private boolean isPrefix;
        private String mRegEx;// 前缀或后缀规则

        public DeleteFileFilter(boolean isPrefix, @NonNull String regEx) {
            this.isPrefix = isPrefix;
            this.mRegEx = regEx;
        }

        @Override
        public boolean accept(File file, String s) {
            return isPrefix ? s.startsWith(mRegEx) : s.endsWith(mRegEx);
        }
    }
}
```

##定义文件删除的异常操作工具类

因为操作文件属于耗时操作，我们使用 **SingleThreadExecutor** 线程池实现如下一个工具类

```
/**
 * Created by xpf on 2017/12/25 :)
 * Function:
 */

public class DeleteUtil {

    private static final ExecutorService executor = Executors.newSingleThreadExecutor();

    /**
     * 删除指定目录指定前后缀的文件
     *
     * @param dirPath
     * @param isPrefix
     * @param regEx
     */
    public static void delete(String dirPath, boolean isPrefix, String regEx) {
        executor.execute(new DeleteRunnable(dirPath, isPrefix, regEx));
    }
}
```

##举例

例如我们要删除要删除SD卡下的Download目录下的所有以.png后缀结尾的文件，只需要这样写：

```
DeleteUtil.delete("/sdcard/Download/", false, ".png");
```

是不是感觉很方便，0.0~ 可以收藏了，以后当工具类来使用。

如果有不懂的地方或者不对的地方，欢迎评论交流，谢谢！

最后附上下载Demo的地址:http://download.csdn.net/download/xinpengfei521/10172262

若在使用过程中遇到什么问题，或有好提议，欢迎在公众号“CodingAndroid”中提出
![这里写图片描述](http://img.blog.csdn.net/20170911223615073)