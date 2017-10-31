#Android追加换行写入内容到txt文件中

实现思路为：使用Context.MODE_APPEND属性将每次内容的写入到上次内容的尾部，然后在每次写入内容完成之后写入一个回车换行符**fos.write("\r\n".getBytes());**即可。

```
/**
 * Created by xpf on 2017/10/31 :)
 * Function:
 */

public class FileHelper {

    private Context mContext;

    public FileHelper(Context mContext) {
        this.mContext = mContext;
    }

    /**
     * 定义文件保存的方法，写入到文件中，所以是输出流
     */
    public void save(String adNum, String time) {
        String content = "广告位编号：" + adNum + "，播放时长：" + time;
        FileOutputStream fos = null;
        try {
            // Context.MODE_PRIVATE私有权限，Context.MODE_APPEND追加写入到已有内容的后面
            fos = mContext.openFileOutput(getFileName(), Context.MODE_APPEND);
            fos.write(content.getBytes());
            fos.write("\r\n".getBytes());//写入换行
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fos != null) {
                try {
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    /**
     * 定义文件读取的方法
     */
    public String read(String filename) throws IOException {
        FileInputStream fis = mContext.openFileInput(filename);
        byte[] buff = new byte[1024];
        StringBuilder sb = new StringBuilder("");
        int len = 0;
        while ((len = fis.read(buff)) > 0) {
            sb.append(new String(buff, 0, len));
        }
        fis.close();
        return sb.toString();
    }

    /**
     * get file name such as 20171031.txt
     *
     * @return
     */
    private String getFileName() {
        return TimeUtil.getCurrentDay() + ".txt";
    }
}
```

转载请注明出处，谢谢！
