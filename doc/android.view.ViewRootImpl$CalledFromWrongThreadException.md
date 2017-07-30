#android.view.ViewRootImpl$CalledFromWrongThreadException...

在使用EventBus时报如下异常：

```
E/EventBus: Could not dispatch event: class com.model.EventBusMsg to subscribing class class com.anloq.activity.SettingActivity
                                                           android.view.ViewRootImpl$CalledFromWrongThreadException: Only the original thread that created a view hierarchy can touch its views.
                                                               at android.view.ViewRootImpl.checkThread(ViewRootImpl.java:6096)
                                                               at android.view.ViewRootImpl.requestLayout(ViewRootImpl.java:824)
```

上面大致说的意思是EventBus再设置处理UI时只能在UI线程中，我使用的是 **BACKGROUND**

解决办法：1.改为 **MAIN**；  
		 2.处理UI时加上runOnUiThread即可；