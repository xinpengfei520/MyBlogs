
#Android点亮屏幕并解锁

需求：现在在做一个app要求有唤醒屏幕并解锁的需求，参考了网上的许多的博客，感觉前篇一律，有的手机上好使，在有的手机上不好使，参考了微信的视频呼叫唤醒屏幕，可以点亮手机屏幕，对于有上滑手势解锁的手机只能点亮屏幕不能解锁屏幕，后来经过尝试，当前应用中有一个**锁屏弹起界面** 的权限，默认是关闭的，打开这个权限之后就可以点亮屏幕并弹出界面了(有上滑手势解锁的手机)，后台经过尝试现在可以点亮屏幕并解锁了...

##代码如下：

```
	/**
     * 唤醒手机屏幕并解锁
     */
    public static void wakeUpAndUnlock() {
        // 获取电源管理器对象
        PowerManager pm = (PowerManager) MyApplication.getContext()
                .getSystemService(Context.POWER_SERVICE);
        boolean screenOn = pm.isScreenOn();
        if (!screenOn) {
            // 获取PowerManager.WakeLock对象,后面的参数|表示同时传入两个值,最后的是LogCat里用的Tag
            PowerManager.WakeLock wl = pm.newWakeLock(
                    PowerManager.ACQUIRE_CAUSES_WAKEUP |
                            PowerManager.SCREEN_BRIGHT_WAKE_LOCK, "bright");
            wl.acquire(10000); // 点亮屏幕
            wl.release(); // 释放
        }
        // 屏幕解锁
        KeyguardManager keyguardManager = (KeyguardManager) MyApplication.getContext()
                .getSystemService(KEYGUARD_SERVICE);
        KeyguardManager.KeyguardLock keyguardLock = keyguardManager.newKeyguardLock("unLock");
        // 屏幕锁定
        keyguardLock.reenableKeyguard();
        keyguardLock.disableKeyguard(); // 解锁
    }
```

注意：好多写法都是```wl.acquire()``` 这样写的，在有的手机上不好使，后来我这样写```wl.acquire(10000); // 点亮屏幕```然后就好使了，我的理解是这样的：是不是时间太短了，刚唤醒屏幕就释放掉了会有问题，由于水平有限不对的地方请指出。

测试机型：xiaomi 5, OPPO R9s plus.

其他机型请自测。
		

##不要忘了添加如下权限

```
	<uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
```

Thanks.

参考文章：http://blog.csdn.net/glen1943/article/details/8671793