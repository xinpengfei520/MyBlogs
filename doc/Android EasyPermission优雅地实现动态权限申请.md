
#Android EasyPermission优雅地实现动态权限申请

Google开源的动态权限适配库，用起来比较简洁和清晰，总体还不错，如果喜欢链式调用的就用 **RxPermission** 或者 **AndPermission** 都是不错的选择。

##一、权限等级和权限组介绍

权限主要分为 **normal** 、**dangerous**、**signature** 和 **signatureOrSystem**四个等级，一般情况下，我们只需要了解前两种权限的使用就可以了，后面的是属于系统级签名的权限。

##二、普通权限和危险权限

普通权限只需要在AndroidMenifest.xml文件中申请即可，危险权限需要到代码中进行动态申请一般是成对成组地去申请，所以叫权限组

普通权限对应表：

| Name| Permission |
| ---- | ---- |  
|ACCESS_LOCATION_EXTRA_COMMANDS|ACCESS_NETWORK_STATE|  
|ACCESS_NOTIFICATION_POLICY|ACCESS_WIFI_STATE|  
|BLUETOOTH|BLUETOOTH_ADMIN|  
|BROADCAST_STICKY|CHANGE_NETWORK_STATE|  
|CHANGE_WIFI_MULTICAST_STATE|CHANGE_WIFI_STATE|  
|DISABLE_KEYGUARD|EXPAND_STATUS_BAR|  
|GET_PACKAGE_SIZE|INSTALL_SHORTCUT|  
|INTERNET|KILL_BACKGROUND_PROCESSES|  
|MODIFY_AUDIO_SETTINGS|NFC|  
|READ_SYNC_SETTINGS|READ_SYNC_STATS|  
|RECEIVE_BOOT_COMPLETED|REORDER_TASKS|  
|REQUEST_IGNORE_BATTERY_OPTIMIZATIONS|REQUEST_INSTALL_PACKAGES|  
|SET_ALARM|SET_TIME_ZONE|  
|SET_WALLPAPER|SET_WALLPAPER_HINTS|  
|TRANSMIT_IR|UNINSTALL_SHORTCUT|  
|USE_FINGERPRINT|VIBRATE|  
|WAKE_LOCK|WRITE_SYNC_SETTINGS|  

危险权限对应表：

<table>
<thead>
<tr>
  <th align="left">权限组</th>
  <th align="left">权限</th>
</tr>
</thead>
<tbody><tr>
  <td align="left">CALENDAR</td>
  <td align="left">READ_CALENDAR 
 WRITE_CALENDAR</td>
</tr>
<tr>
  <td align="left">CAMERA</td>
  <td align="left">CAMERA</td>
</tr>
<tr>
  <td align="left">CONTACTS</td>
  <td align="left">READ_CONTACTS 
 WRITE_CONTACTS 
 GET_ACCOUNTS</td>
</tr>
<tr>
  <td align="left">LOCATION</td>
  <td align="left">ACCESS_FINE_LOCATION  
 ACCESS_COARSE_LOCATION</td>
</tr>
<tr>
  <td align="left">MICROPHONE</td>
  <td align="left">RECORD_AUDIO</td>
</tr>
<tr>
  <td align="left">PHONE</td>
  <td align="left">READ_PHONE_STATE 
 CALL_PHONE 
 READ_CALL_LOG 
 WRITE_CALL_LOG 
 ADD_VOICEMAIL 
 USE_SIP 
 PROCESS_OUTGOING_CALLS</td>
</tr>
<tr>
  <td align="left">SENSORS</td>
  <td align="left">BODY_SENSORS</td>
</tr>
<tr>
  <td align="left">SMS</td>
  <td align="left">SEND_SMS 
 RECEIVE_SMS 
 READ_SMS 
 RECEIVE_WAP_PUSH 
 RECEIVE_MMS</td>
</tr>
<tr>
  <td align="left">STORAGE</td>
  <td align="left">READ_EXTERNAL_STORAGE 
 WRITE_EXTERNAL_STORAGE</td>
</tr>
</tbody></table>

下面简单介绍一下EasyPermission的简单使用，更多详细使用请参考：https://github.com/googlesamples/easypermissions

##1.添加依赖库

```
compile 'pub.devrel:easypermissions:1.0.1'
```

##2.定义需要动态申请的权限集合

```
	/**
     * 需要申请的权限数组
     */
    protected String[] needPermissions = {
            Manifest.permission.WRITE_EXTERNAL_STORAGE,
            Manifest.permission.READ_EXTERNAL_STORAGE,
    };
```

##3.重写回调方法

```
	@Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);

        // Forward results to EasyPermissions
        EasyPermissions.onRequestPermissionsResult(requestCode, permissions, grantResults, this);
    }
```

##4.授权成功和失败的回调

```
	@AfterPermissionGranted(RC_CAMERA_AND_LOCATION)
	private void methodRequiresTwoPermission() {
	    String[] perms = {Manifest.permission.CAMERA, Manifest.permission.ACCESS_FINE_LOCATION};
	    if (EasyPermissions.hasPermissions(this, perms)) {
	        // Already have permission, do the thing
	        // ...
	    } else {
	        // Do not have permissions, request them now
	        EasyPermissions.requestPermissions(this, getString(R.string.camera_and_location_rationale),
	                RC_CAMERA_AND_LOCATION, perms);
	    }
	}
```

##第二种写法：

```
	EasyPermissions.requestPermissions(
        new PermissionRequest.Builder(this, RC_CAMERA_AND_LOCATION, perms)
                .setRationale(R.string.camera_and_location_rationale)
                .setPositiveButtonText(R.string.rationale_ask_ok)
                .setNegativeButtonText(R.string.rationale_ask_cancel)
                .setTheme(R.style.my_fancy_style)
                .build());
```

重写授权成功和失败的回调

```
	@Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);

        // Forward results to EasyPermissions
        EasyPermissions.onRequestPermissionsResult(requestCode, permissions, grantResults, this);
    }

    @Override
    public void onPermissionsGranted(int requestCode, List<String> list) {
        // Some permissions have been granted
        // ...
    }

    @Override
    public void onPermissionsDenied(int requestCode, List<String> list) {
        // Some permissions have been denied
        // ...
    }
```

##更多使用请参考：

[https://github.com/googlesamples/easypermissions](https://github.com/googlesamples/easypermissions)

##另外再推荐几个不错的处理动态权限的开源库：

前面的star基本都是几k,后面的都是几百的star

[https://github.com/tbruyelle/RxPermissions](https://github.com/tbruyelle/RxPermissions)

[https://github.com/hongyangAndroid/MPermissions](https://github.com/hongyangAndroid/MPermissions)

[https://github.com/yanzhenjie/AndPermission](https://github.com/yanzhenjie/AndPermission)

[https://github.com/permissions-dispatcher/PermissionsDispatcher](https://github.com/permissions-dispatcher/PermissionsDispatcher)

[https://github.com/jokermonn/permissions4m](https://github.com/jokermonn/permissions4m)

[https://github.com/lovedise/PermissionGen](https://github.com/lovedise/PermissionGen)

[https://github.com/k0shk0sh/PermissionHelper](https://github.com/k0shk0sh/PermissionHelper)

支持Kotlin:[https://github.com/googlesamples/android-RuntimePermissions](https://github.com/googlesamples/android-RuntimePermissions)

[https://github.com/mylhyl/AndroidAcp](https://github.com/mylhyl/AndroidAcp)

[https://github.com/ParkSangGwon/TedPermission](https://github.com/ParkSangGwon/TedPermission)

[https://github.com/yewei02538/HiPermission](https://github.com/yewei02538/HiPermission)

[https://github.com/PEXPlugins/PermissionsEx](https://github.com/PEXPlugins/PermissionsEx)

[https://github.com/dfqin/PermissionGrantor](https://github.com/dfqin/PermissionGrantor)

##更多Google官网文档关于动态权限的说明

[https://developer.android.com/guide/topics/security/permissions.html](https://developer.android.com/guide/topics/security/permissions.html)

[https://developer.android.com/training/permissions/requesting.html](https://developer.android.com/training/permissions/requesting.html)

[https://developer.android.com/training/permissions/best-practices.html](https://developer.android.com/training/permissions/best-practices.html)