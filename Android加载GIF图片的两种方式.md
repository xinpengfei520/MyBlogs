#Android加载GIF图片的两种方式

##方式一：使用第三开源框架直接在布局文件中加载gif

###1.在工程的build.gradle中添加如下

```
buildscript {
        repositories {
            mavenCentral()
        }
    }
    allprojects {
        repositories {
            mavenCentral()
        }
    }
```

###2.在app的build.gradle中添加依赖

```compile 'pl.droidsonroids.gif:android-gif-drawable:1.2.1'```

###3.布局文件中就可以直接写你需要加载的gif图片即可

```
<pl.droidsonroids.gif.GifImageView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/gif2" />
```

##方式二：使用Glide加载gif

这种方式就比较简单了，直接看代码↓

###1.添加Glide图片加载框架依赖

```compile 'com.github.bumptech.glide:glide:3.7.0'```

###2.布局文件中写一个普通的Imageview

```
<ImageView
        android:id="@+id/ivGif"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />
```

###3.代码中直接加载本地的gif图片到Imageview上即可

```
Glide.with(MainActivity.this).load(R.drawable.gif1)
                        .diskCacheStrategy(DiskCacheStrategy.ALL).into(ivGif);```

以上就是加载gif图片的两种方式，我写的都是加载本地的gif，加载网络的gif图片应该也差不多一样，自己尝试吧...

代码比较简单，为了照顾初学者，需要的同学 ![戳这里](https://github.com/xinpengfei520/Android-GIF-Loader.git) 下载

Thanks all.

2017.5.30日  农历：五月初五 端午节

最后祝大家端午节快乐！！！大家记得吃粽子哦 ：)