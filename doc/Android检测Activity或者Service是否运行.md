
#Android检测Activity或者Service是否运行

需求：假设我们的APP有3个页面AActivity,BActivity,CActivity,我们的APP需要一直运行在前台（特殊设备），要求实现一个监控服务，来监视APP是否运行，如果有3个页面都不运行了就说明这个APP已经挂掉了，否则说明APP在运行状态，不做处理，挂掉之后，我们需要重新启动App来让它继续处理运行状态，对外暴露一个来停止监控服务的广播，这样我们想停止监控服务时，发送一个广播即可。

思路：实现一个双进程的监控服务，服务中写一个定时器 **Timer** 来重复进行检测是否正在运行，如果否就直接重新启动APP。

##1.定义一个监控服务

```
package com.anloq.nfcservice;

import android.app.Service;
import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
import android.content.IntentFilter;
import android.os.IBinder;
import android.util.Log;

import com.anloq.MyApplication;
import com.anloq.activity.AdActivity;
import com.anloq.utils.DetectionASUtils;

import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Locale;
import java.util.Timer;
import java.util.TimerTask;

/**
 * Created by xpf on 2017/6/3 :)
 * 检测APP页面是否一直运行,不运行就直接启动
 */

public class MonitoringService extends Service {

    private final static String TAG = "MonitoringService";

    private BroadcastReceiver broadcastReceiver = new BroadcastReceiver() {
        @Override
        public void onReceive(Context context, Intent intent) {
            if ("kill_self".equals(intent.getAction())) {
                Log.e(TAG, "onReceive:杀死自己的进程！");
                killMyselfPid(); // 杀死自己的进程
            }
        }
    };

    private Timer timer = new Timer();
    private TimerTask task = new TimerTask() {
        @Override
        public void run() {
            checkIsAlive();
        }
    };

    /**
     * 检测应用是否活着
     */
    private void checkIsAlive() {
        String format = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss",
                Locale.CHINA).format(new Date());
        Log.e(TAG, "CustodyService Run: " + format);

        boolean AIsRunning = CheckUtil.isClsRunning(
                MonitoringService.this, "com.xpf.monitor", "com.xpf.monitor.activity.AActivity");
        boolean BIsRunning = CheckUtil.isClsRunning(
                MonitoringService.this, "com.xpf.monitor", "com.xpf.monitor.activity.BActivity");
        boolean b = (AIsRunning || BIsRunning);
        boolean CIsRunning = CheckUtil.isClsRunning(
                MonitoringService.this, "com.xpf.monitor", "com.xpf.monitor.activity.CActivity");

        Log.e(TAG, "AIsRunning || BIsRunning is running:" + b + ",CIsRunning:" + CIsRunning);

        if (!CIsRunning) {
            if (!b) { //如果界面挂掉直接启动AActivity
                Intent intent = new Intent();
                intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
                intent.setClass(MonitoringService.this, AActivity.class);
                startActivity(intent);
            }
        }
    }


    @Override
    public void onCreate() {
        super.onCreate();
        Log.e(TAG, "onCreate: 启动监控服务! ");
        IntentFilter intentFilter = new IntentFilter();
        intentFilter.addAction("kill_self");
        registerReceiver(broadcastReceiver, intentFilter);
        timer.schedule(task, 0, 10000);// 设置检测的时间周期(毫秒数)
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        return START_STICKY;
    }

    @Override
    public IBinder onBind(Intent arg0) {
        return null;
    }

    /**
     * 杀死自身的进程
     */
    private void killMyselfPid() {
        int pid = android.os.Process.myPid();
        String command = "kill -9 " + pid;
        Log.e(TAG, "killMyselfPid: " + command);
        stopService(new Intent(MonitoringService.this, MonitoringService.class));
        try {
            Runtime.getRuntime().exec(command);
            System.exit(0);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        unregisterReceiver(broadcastReceiver);
        if (task != null) {
            task.cancel();
        }
        if (timer != null) {
            timer.cancel();
        }
    }
}
```

##2.注册双进程Service

```
		<service
            android:name="com.xpf.monitor.MonitoringService"
            android:enabled="true"
            android:label="MonitoringService"
            android:process=":gray">
            <intent-filter>
                <action android:name="android.intent.action.RESPOND_VIA_MESSAGE" />
            </intent-filter>
        </service>
```

##3.检测是否活着的工具类CheckUtil

```
public class CheckUtil {
    //检测service是否在运行
    public static boolean isServiceWorked(Context context, String serviceName) {
        ActivityManager myManager = (ActivityManager) context.getSystemService(Context.ACTIVITY_SERVICE);
        ArrayList<ActivityManager.RunningServiceInfo> runningService = (ArrayList<ActivityManager.RunningServiceInfo>) myManager.getRunningServices(Integer.MAX_VALUE);
        for (int i = 0; i < runningService.size(); i++) {
            if (runningService.get(i).service.getClassName().toString().equals(serviceName)) {
                return true;
            }
        }
        return false;
    }

    //检测activity是否存在再栈顶
    public static boolean isForeground(Context context, String PackageName) {
        ActivityManager myManager = (ActivityManager) context.getSystemService(Context.ACTIVITY_SERVICE);
        List<ActivityManager.RunningTaskInfo> task = myManager.getRunningTasks(1);
        ComponentName componentInfo = task.get(0).topActivity;
        if (componentInfo.getPackageName().equals(PackageName))
            return true;
        return false;
    }

    /**
     * 判断某个app进程是否在运行
     *
     * @param context
     * @param appInfo
     * @return
     */
    public static boolean isRunningProcess(Context context, String appInfo) {
        ActivityManager myManager = (ActivityManager) context.getSystemService(Context.ACTIVITY_SERVICE);
        List<ActivityManager.RunningAppProcessInfo> runningAppPs = myManager.getRunningAppProcesses();
        if (runningAppPs != null && runningAppPs.size() > 0) {
            if (runningAppPs.contains(appInfo)) {
                return true;
            }
        }
        return false;
    }

    /**
     * 判断一个Activity是否正在运行
     *
     * @param pkg     pkg为应用包名
     * @param cls     cls为类名eg
     * @param context
     * @return
     */
    public static boolean isClsRunning(Context context, String pkg, String cls) {
        ActivityManager am = (ActivityManager) context.getSystemService(Context.ACTIVITY_SERVICE);
        List<ActivityManager.RunningTaskInfo> tasks = am.getRunningTasks(1);
        ActivityManager.RunningTaskInfo task = tasks.get(0);
        if (task != null) {
            return TextUtils.equals(task.topActivity.getPackageName(), pkg) &&
                    TextUtils.equals(task.topActivity.getClassName(), cls);
        }
        return false;
    }
}
```

##4.MainActivity中启动监控服务

```
	Intent intent = new Intent(MainActivity.this, MonitoringService.class);
    intent.setAction("android.intent.action.RESPOND_VIA_MESSAGE");
    startService(intent);
```

##5.停止监控服务

发送一个杀死进程广播即可，action值如下

```
	Intent intent = new Intent();
    intent.setAction("kill_self");
    sendOrderedBroadcast(intent, null);
```

好了，今天就分享到这里了。。。

欢迎评论和点赞！！！