#Android截屏的几种实现

##方式一

```
	public static Bitmap capture(Activity activity) {
        activity.getWindow().getDecorView().setDrawingCacheEnabled(true);
        Bitmap bmp = activity.getWindow().getDecorView().getDrawingCache();
        return bmp;
    }
```

此种方式比较简单只需传入当前要截取屏幕的Activity对象即可，不需要添加任何权限，后续可将截图的bitmap保存到本地即可；

缺点：无法截取WebView页面，截屏后是白屏！

##方式二

使用adb Shell命令截屏

例如：

```adb shell screencap -p /sdcard/sreenshot1.png```

命令格式：adb shell screencap -p + 文件路径 + 文件名

##方式三

从Android 5.0开始 Google开放了截屏的api

截取方式如下：

1.在Activity中开启截屏服务

```
	if (Build.VERSION.SDK_INT >= 21) {
            startActivityForResult(
                    ((MediaProjectionManager) getSystemService("media_projection")).createScreenCaptureIntent(),1);
        } else {
            Log.e("TAG", "版本过低,无法截屏");
        }
```

2.重写**onActivityResult**方法

```
	@Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        switch (requestCode) {
            case REQUEST_MEDIA_PROJECTION: {
                if (resultCode == -1 && data != null) {
                    parseData(data);
                }
            }
        }
    }

	private void parseData(Intent data){
		MediaProjection mMediaProjection = (MediaProjectionManager).getSystemService(
                Context.MEDIA_PROJECTION_SERVICE).getMediaProjection(Activity.RESULT_OK,data);
            ImageReader mImageReader = ImageReader.newInstance(
                    getScreenWidth(),
                    getScreenHeight(),
                    PixelFormat.RGBA_8888,1);

		VirtualDisplay mVirtualDisplay = mMediaProjection.createVirtualDisplay("screen-mirror",
                getScreenWidth(),
                getScreenHeight(),
                Resources.getSystem().getDisplayMetrics().densityDpi,
                DisplayManager.VIRTUAL_DISPLAY_FLAG_AUTO_MIRROR,
                mImageReader.getSurface(), null, null);

		Handler handler = new Handler();
        handler.postDelayed(new Runnable() {
           @Override
           public void run() {
               Image image = mImageReader.acquireLatestImage();
               // TODO 将image保存到本地即可
           }
        }, 300);

		mVirtualDisplay.release();
		mVirtualDisplay = null;
	}

```

更多参考文章：

http://www.jb51.net/article/119881.htm

https://www.2cto.com/kf/201602/488933.html

http://blog.csdn.net/buptgshengod/article/details/39155979

下面这个库封装了cmd截屏和5.0以上的截屏：

https://github.com/Android-ScreenShot/AndroidScreenShotService