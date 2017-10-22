#《Android源码设计模式》学习笔记之ImageLoader

需求：设计一个图片加载工具类。
要求：职责单一、可扩展性强、实现三级缓存，遵循开闭原则。

##1.改造前原始代码

```
package com.anloq.sdk.imageloader;

import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.util.LruCache;
import android.widget.ImageView;

import java.net.HttpURLConnection;
import java.net.URL;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

/**
 * Created by xpf on 2017/10/22 :)
 * Function:
 */

public class ImageLoader {

    // 图片缓存
    LruCache<String, Bitmap> mImageCache;
    // 线程池，线程池数量为CPU的数量
    ExecutorService mExecutorService = Executors.newFixedThreadPool(
            Runtime.getRuntime().availableProcessors());

    public ImageLoader() {
        initImageCache();
    }

    /**
     * 初始化图片缓存大小
     */
    private void initImageCache() {
        // 计算可使用的最大内存
        final int maxMemory = (int) (Runtime.getRuntime().maxMemory() / 1024);
        // 取1/4的可用内存作为缓存
        final int cacheSize = maxMemory / 4;
        mImageCache = new LruCache<String, Bitmap>(cacheSize) {
            @Override
            protected int sizeOf(String key, Bitmap value) {
                return value.getRowBytes() * value.getHeight() / 1024;
            }
        };
    }

    /**
     * 加载显示图片
     *
     * @param url
     * @param imageView
     */
    public void displayImage(final String url, final ImageView imageView) {
        imageView.setTag(url);
        mExecutorService.submit(new Runnable() {
            @Override
            public void run() {
                Bitmap bitmap = downloadImage(url);
                if (bitmap == null) return;
                if (imageView.getTag().equals(url)) {
                    imageView.setImageBitmap(bitmap);
                }
                mImageCache.put(url, bitmap);
            }
        });

    }

    /**
     * 下载图片
     *
     * @param imageUrl
     * @return
     */
    private Bitmap downloadImage(String imageUrl) {
        Bitmap bitmap = null;
        try {
            URL url = new URL(imageUrl);
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            bitmap = BitmapFactory.decodeStream(conn.getInputStream());
            conn.disconnect();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return null;
    }
}
```

##2.遵循单一原则将原始类分为加载和缓存两个类（功能）

###2.1.图片加载类为：

```
package com.anloq.sdk.imageloader;

import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.widget.ImageView;

import java.net.HttpURLConnection;
import java.net.URL;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

/**
 * Created by xpf on 2017/10/22 :)
 * Function:
 */

public class ImageLoader {

    // 图片缓存
    ImageCache mImageCache = new ImageCache();
    // 线程池，线程池数量为CPU的数量
    ExecutorService mExecutorService = Executors.newFixedThreadPool(
            Runtime.getRuntime().availableProcessors());

    /**
     * 加载显示图片
     *
     * @param url
     * @param imageView
     */
    public void displayImage(final String url, final ImageView imageView) {
        // 优先从缓存中加载
        Bitmap bitmap = mImageCache.get(url);
        if (bitmap != null) {
            imageView.setImageBitmap(bitmap);
            return;
        }
        imageView.setTag(url);
        mExecutorService.submit(new Runnable() {
            @Override
            public void run() {
                Bitmap bitmap = downloadImage(url);
                if (bitmap == null) return;
                if (imageView.getTag().equals(url)) {
                    imageView.setImageBitmap(bitmap);
                }
                mImageCache.put(url, bitmap);
            }
        });
    }

    /**
     * 下载图片
     *
     * @param imageUrl
     * @return
     */
    private Bitmap downloadImage(String imageUrl) {
        Bitmap bitmap = null;
        try {
            URL url = new URL(imageUrl);
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            bitmap = BitmapFactory.decodeStream(conn.getInputStream());
            conn.disconnect();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return null;
    }
}
```

###2.2.缓存类为

```
package com.anloq.sdk.imageloader;

import android.graphics.Bitmap;
import android.util.LruCache;

/**
 * Created by xpf on 2017/10/22 :)
 * Function:图片缓存类
 */

public class ImageCache {
    // 图片LRU缓存
    LruCache<String, Bitmap> mImageCache;

    public ImageCache() {
        initImageCache();
    }

    /**
     * 初始化图片缓存大小
     */
    private void initImageCache() {
        // 计算可使用的最大内存
        final int maxMemory = (int) (Runtime.getRuntime().maxMemory() / 1024);
        // 取1/4的可用内存作为缓存
        final int cacheSize = maxMemory / 4;
        mImageCache = new LruCache<String, Bitmap>(cacheSize) {
            @Override
            protected int sizeOf(String key, Bitmap value) {
                return value.getRowBytes() * value.getHeight() / 1024;
            }
        };
    }

    public void put(String url, Bitmap bitmap) {
        mImageCache.put(url, bitmap);
    }

    public Bitmap get(String url) {
        return mImageCache.get(url);
    }
}
```

##3.提高扩展性，增加SD卡缓存

以上将代码的功能分开了，逻辑更清晰了，职责也单一了，但是可扩展性还是比较差，接下来进行增加SD卡缓存。


###3.1增加SD卡缓存类

```
package com.anloq.sdk.imageloader;

import android.graphics.Bitmap;
import android.graphics.BitmapFactory;

import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

/**
 * Created by xpf on 2017/10/22 :)
 * Function:
 */

public class DiskCache {
    static String cacheDir = "/sdcard/cache/image/";

    /**
     * 从SD卡中读取
     *
     * @param url
     * @return
     */
    public Bitmap get(String url) {
        return BitmapFactory.decodeFile(cacheDir + url);
    }

    /**
     * 缓存到SD卡中
     *
     * @param url
     * @param bmp
     */
    public void put(String url, Bitmap bmp) {
        FileOutputStream fos = null;
        try {
            fos = new FileOutputStream(cacheDir + url);
            bmp.compress(Bitmap.CompressFormat.PNG, 100, fos);
        } catch (FileNotFoundException e) {
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
}
```

###3.2ImageLoader中增加一个boolean值来设置使用哪种缓存方式

```
package com.anloq.sdk.imageloader;

import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.widget.ImageView;

import java.net.HttpURLConnection;
import java.net.URL;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

/**
 * Created by xpf on 2017/10/22 :)
 * Function:
 */

public class ImageLoader {

    // 内存缓存
    ImageCache mImageCache = new ImageCache();
    // SD卡缓存
    DiskCache mDiskCache = new DiskCache();
    // 是否使用SD卡缓存
    boolean isUseDiskCache = false;
    // 线程池，线程池数量为CPU的数量
    ExecutorService mExecutorService = Executors.newFixedThreadPool(
            Runtime.getRuntime().availableProcessors());

    /**
     * 加载显示图片
     *
     * @param url
     * @param imageView
     */
    public void displayImage(final String url, final ImageView imageView) {
        // 优先从缓存中加载
        Bitmap bitmap = isUseDiskCache ? mImageCache.get(url) : mDiskCache.get(url);
        if (bitmap != null) {
            imageView.setImageBitmap(bitmap);
            return;
        }
        imageView.setTag(url);
        mExecutorService.submit(new Runnable() {
            @Override
            public void run() {
                Bitmap bitmap = downloadImage(url);
                if (bitmap == null) return;
                if (imageView.getTag().equals(url)) {
                    imageView.setImageBitmap(bitmap);
                }
                mImageCache.put(url, bitmap);
            }
        });
    }

    /**
     * 下载图片
     *
     * @param imageUrl
     * @return
     */
    private Bitmap downloadImage(String imageUrl) {
        Bitmap bitmap = null;
        try {
            URL url = new URL(imageUrl);
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            bitmap = BitmapFactory.decodeStream(conn.getInputStream());
            conn.disconnect();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return null;
    }

    /**
     * 设置是否使用SD卡缓存
     *
     * @param useDiskCache
     */
    public void setUseDiskCache(boolean useDiskCache) {
        isUseDiskCache = useDiskCache;
    }
}
```

##4.进一步改造，使用双缓存，优先使用内存加载，如果无再使用SD卡缓存

以上代码修改虽然增加了SD卡缓存，但是为了节省用户的流量及加载速度我们应该设计成优先使用内存加载，如果无再使用SD卡缓存。

###4.1增加双缓存类

```
package com.anloq.sdk.imageloader;

import android.graphics.Bitmap;

/**
 * Created by xpf on 2017/10/22 :)
 * Function:
 */

public class DoubleCache {

    ImageCache mMemoryCache = new ImageCache();
    DiskCache mDiskCache = new DiskCache();

    /**
     * 优先使用内存加载，如果无再使用SD卡缓存
     *
     * @param url
     * @return
     */
    public Bitmap get(String url) {
        Bitmap bitmap = mMemoryCache.get(url);
        if (bitmap == null) {
            bitmap = mDiskCache.get(url);
        }
        return bitmap;
    }

    /**
     * 将图片缓存到内存和SD卡中
     *
     * @param url
     * @param bitmap
     */
    public void put(String url, Bitmap bitmap) {
        mMemoryCache.put(url, bitmap);
        mDiskCache.put(url, bitmap);
    }
}
```

###4.2ImageLoader增加双缓存配置

```
package com.anloq.sdk.imageloader;

import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.widget.ImageView;

import java.net.HttpURLConnection;
import java.net.URL;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

/**
 * Created by xpf on 2017/10/22 :)
 * Function:
 */

public class ImageLoader {

    // 内存缓存
    ImageCache mImageCache = new ImageCache();
    // SD卡缓存
    DiskCache mDiskCache = new DiskCache();
    // 双缓存
    DoubleCache mDoubleCache = new DoubleCache();
    // 是否使用SD卡缓存
    boolean isUseDiskCache = false;
    // 是否使用双缓存
    boolean isUseDoubleCache = false;
    // 线程池，线程池数量为CPU的数量
    ExecutorService mExecutorService = Executors.newFixedThreadPool(
            Runtime.getRuntime().availableProcessors());

    /**
     * 加载显示图片
     *
     * @param url
     * @param imageView
     */
    public void displayImage(final String url, final ImageView imageView) {
        // 优先从缓存中加载
        Bitmap bitmap = null;
        if (isUseDoubleCache) {
            bitmap = mDoubleCache.get(url);
        } else if (isUseDiskCache) {
            bitmap = mDiskCache.get(url);
        } else {
            bitmap = mImageCache.get(url);
        }
        if (bitmap != null) {
            imageView.setImageBitmap(bitmap);
            return;
        }
        imageView.setTag(url);
        mExecutorService.submit(new Runnable() {
            @Override
            public void run() {
                Bitmap bitmap = downloadImage(url);
                if (bitmap == null) return;
                if (imageView.getTag().equals(url)) {
                    imageView.setImageBitmap(bitmap);
                }
                mImageCache.put(url, bitmap);
            }
        });
    }

    /**
     * 下载图片
     *
     * @param imageUrl
     * @return
     */
    private Bitmap downloadImage(String imageUrl) {
        Bitmap bitmap = null;
        try {
            URL url = new URL(imageUrl);
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            bitmap = BitmapFactory.decodeStream(conn.getInputStream());
            conn.disconnect();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return null;
    }

    /**
     * 设置是否使用SD卡缓存
     *
     * @param useDiskCache
     */
    public void setUseDiskCache(boolean useDiskCache) {
        isUseDiskCache = useDiskCache;
    }

    /**
     * 设置是否使用双缓存
     *
     * @param useDoubleCache
     */
    public void setUseDoubleCache(boolean useDoubleCache) {
        isUseDoubleCache = useDoubleCache;
    }
}
```

以上改造总算可以了，但是这样每次增加缓存策略都要修改源代码，这样很有可能引入bug,所以我们的原则是要对修改关闭，对扩展开放，这样以后有新需求的时候我们就可以使用扩展的方法来实现。

##5.抽象公共方法的接口

###5.1接口抽取

```
package com.anloq.sdk.imageloader;

import android.graphics.Bitmap;

/**
 * Created by xpf on 2017/10/22 :)
 * Function:
 */

public interface ImageCache {
    Bitmap get(String url);

    void put(String url, Bitmap bitmap);
}
```

###5.2ImageLoader注入接口的实现类

```
package com.anloq.sdk.imageloader;

import android.graphics.Bitmap;
import android.graphics.BitmapFactory;
import android.widget.ImageView;

import java.net.HttpURLConnection;
import java.net.URL;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

/**
 * Created by xpf on 2017/10/22 :)
 * Function:
 */

public class ImageLoader {

    ImageCache mImageCache = new MemoryCache();

    // 线程池，线程池数量为CPU的数量
    ExecutorService mExecutorService = Executors.newFixedThreadPool(
            Runtime.getRuntime().availableProcessors());

    /**
     * 注入缓存实现
     *
     * @param mImageCache
     */
    public void setmImageCache(ImageCache mImageCache) {
        this.mImageCache = mImageCache;
    }

    /**
     * 加载显示图片
     *
     * @param url
     * @param imageView
     */
    public void displayImage(final String url, final ImageView imageView) {
        Bitmap bitmap = mImageCache.get(url);
        if (bitmap != null) {
            imageView.setImageBitmap(bitmap);
            return;
        }
        // 图片没有缓存提交到线程池中下载
        submitLoadRequest(url, imageView);
    }

    private void submitLoadRequest(final String url, final ImageView imageView) {
        imageView.setTag(url);
        mExecutorService.submit(new Runnable() {
            @Override
            public void run() {
                Bitmap bitmap = downloadImage(url);
                if (bitmap == null) return;
                if (imageView.getTag().equals(url)) {
                    imageView.setImageBitmap(bitmap);
                }
                mImageCache.put(url, bitmap);
            }
        });
    }

    /**
     * 下载图片
     *
     * @param imageUrl
     * @return
     */
    private Bitmap downloadImage(String imageUrl) {
        Bitmap bitmap = null;
        try {
            URL url = new URL(imageUrl);
            HttpURLConnection conn = (HttpURLConnection) url.openConnection();
            bitmap = BitmapFactory.decodeStream(conn.getInputStream());
            conn.disconnect();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return bitmap;
    }

}
```

###5.3内存缓存、SD卡缓存和双缓存分别实现接口

```
package com.anloq.sdk.imageloader;

import android.graphics.Bitmap;

/**
 * Created by xpf on 2017/10/22 :)
 * Function:
 */

public class DoubleCache implements ImageCache {

    ImageCache mMemoryCache = new MemoryCache();
    ImageCache mDiskCache = new DiskCache();

    /**
     * 优先使用内存加载，如果无再使用SD卡缓存
     *
     * @param url
     * @return
     */
    @Override
    public Bitmap get(String url) {
        Bitmap bitmap = mMemoryCache.get(url);
        if (bitmap == null) {
            bitmap = mDiskCache.get(url);
        }
        return bitmap;
    }

    /**
     * 将图片缓存到内存和SD卡中
     *
     * @param url
     * @param bitmap
     */
    @Override
    public void put(String url, Bitmap bitmap) {
        mMemoryCache.put(url, bitmap);
        mDiskCache.put(url, bitmap);
    }
}
```

内存缓存、SD卡缓存实现同上。

##6.外部调用及设置缓存策略

```
private void loadImage() {
        ImageLoader imageLoader = new ImageLoader();
        // 使用内存缓存
        imageLoader.setmImageCache(new MemoryCache());
        // 使用SD卡缓存
        imageLoader.setmImageCache(new DiskCache());
        // 使用双缓存
        imageLoader.setmImageCache(new DoubleCache());
        // 使用自定义的图片缓存
        imageLoader.setmImageCache(new ImageCache() {
            @Override
            public Bitmap get(String url) {
                return null;
            }

            @Override
            public void put(String url, Bitmap bitmap) {

            }
        });
        String imageUrl = "http://p1.meituan.net/160.0.80/xianfu/5e369ac9d6aa54125ad1b6562282b2ca36024.jpeg";
        imageLoader.displayImage(imageUrl, imageView);
    }
```

经过上述代码的重构，我们可以通过**setImageCache(ImageCache cache)**方法注入不同的缓存实现，来使得ImageLoader更简单、健壮、扩展性好灵活性也更高。以上三种缓存图片的具体实现完全不一样，但是它们都有一个共同的特点是都实现了ImageCache接口。当用户需要增加一种新的缓存策略时，我们只需新建一个实现ImageCache接口等待类就可以了，这样就实现了千变万化的缓存策略，并且新扩展的策略不会影响导致ImageLoader类的修改，这正是体现了“对修改关闭，对扩展开放的”原则，所以，我们在设计写代码的时候应该认真地进行思考，希望大家一起思考，一起学习，有所成长！

源码链接：https://github.com/xinpengfei520/MyImageLoader

如果本文对你有帮助，欢迎大家点赞、评论，码字不易，再小的支持也是对博主的莫大鼓励！

今天的分享就到这里注明，谢谢！

声明：文中部分代码摘抄自《Android源码设计模式》一书。

注：本文由博主原创，转载请注明出处，谢谢！