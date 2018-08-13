
<center>点击上面蓝色字体关注<span style="color:#0000ff;">“IT大飞说”</span></center>
<center>置顶公众号（**ID：ITBigFly**）第一时间收到推送</center>

>本篇文章来自「UartAnd7c」的投稿  
原文地址：https://www.jianshu.com/p/9495ba92c138  
原作者简书主页：https://www.jianshu.com/u/621bfbc7c878  
UartAnd7c 是一个在简书坚持原创的 Android 开发者，欢迎大家关注！

![](http://pcobk8jbf.bkt.clouddn.com/FkvOcmPtN2KeiHXtyMSfpvZfIXF7)

>今天把玩公司App发现了一个有趣的现象。启动app白屏。之前没太留意过；在参照大厂App解决白屏的方案后。简单汇总一下。

###前言
冷启动：App第一次启动的时候会加载一个Application进程，首先会创建和初始化Appliation类，然后在加载Activity。
热启动：利用后台已经拥有的进程，从而避免了Application的加载。直接去加载Activity。

解决白屏的方案大致有以下四种

* 加入动画
* 使用占位图
* 使用图片
* 使用windowDisablePreview属性

这里我就描述一下使用占位图和windowDisablePreview属性

####windowDisablePreview属性

```
<style name="SplashTheme" parent="AppTheme">
        <item name="android:windowDisablePreview">true</item>
 </style>
```

在AndroidManifest中設置主Activity的theme值，在主Activity中还原。

```
@Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        setTheme(R.style.AppTheme);
        getWindow().setBackgroundDrawableResource(R.drawable.main_splash_bg);
        super.onCreate(savedInstanceState);
    }
```

运行后发现启动App延迟几秒后跳转到主Activity（有点类似掌上英雄联盟，会让用户误以为是自己手机的问题）。

####使用占位图

```
  <style name="SplashTheme" parent="AppTheme">
        <item name="android:windowBackground">@drawable/shape_launch</item>
        <item name="android:windowFullscreen">true</item>
    </style>
```

```
<layer-list xmlns:android="http://schemas.android.com/apk/res/android"
    android:opacity="opaque">

    <item android:drawable="@android:color/white"/>
    <item>
        <bitmap
            android:src="@mipmap/main_splash_bg"
            android:gravity="fill" />
    </item>

</layer-list>
```

使用windowBackground属性来完成其他操作同windowDisablePreview属性的设置。运行后发现明显白屏问题得到有效解决。

###参考文献

gitHub地址:https://github.com/saulmm/onboarding-examples-android#onboarding-with-a-placeholder  
google论坛:https://plus.google.com/+AndroidDevelopers/posts/Z1Wwainpjhd

温馨提示：点击「阅读原文」可查看原作者简书原文。

**猜你喜欢：**   
[深入浅出Retrofit2.x(一)](https://mp.weixin.qq.com/s/jxBEg7dpFfTNLXNFdvJjrw)  
[深入浅出Retrofit2.x(二)](https://mp.weixin.qq.com/s/c8mdMqjrLvHTgc8oxONiCw)  
[Android 最全 Intent 传递数据姿势](https://mp.weixin.qq.com/s/hVhhDdr3Bo4qLIdOkHsM_Q) 

![](http://pcobk8jbf.bkt.clouddn.com/Fr_08YggSBgBwGhjjZsX8bH8JYNR)
<center><span style="color:#bdbdbd;">欢迎扫码关注我的公众号</span></center>