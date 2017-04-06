#Android Service启动到Activity

从Service启动到Activity基本可以分为两类：  
1.从自己应用的Service启动自己应用的Activity,即显式意图；  
2.从自己应用的Service启动到其他应用的Activity,或者从别的应用的Service启动到自己应用的Activity，即隐式意图；  

##1.显式意图启动

下面为示例写法：

```
	Intent intent = new Intent(EmqttService.this, CallingActivity.class);
    intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
    startActivity(intent);
```

##2.隐式意图启动

需要在manifest.xml中为该Activity配置```<intent-filter>```标签，为其配置action和category

```
    <activity android:name=".CallingActivity" >  
            <intent-filter>  
                <action android:name="com.call"></action>  
                <category android:name="android.intent.category.DEFAULT"/>  
            </intent-filter>  
    </activity>  
```

在要启动的Service里这样写：

```
	Intent i = new Intent("com.call");  
	i.addCategory(Intent.CATEGORY_DEFAULT);  
	i.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);  
	startActivity(i);
```

注意```“com.call”```为要启动Activity的Action.

Thanks all.