
#Android设置Activity全屏的两种方式

##1.代码中

在Activity的onCreate()方法中设置两个Flag：

```
super.onCreate(savedInstanceState);
this.requestWindowFeature(Window.FEATURE_NO_TITLE);
this.getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN,
                WindowManager.LayoutParams.FLAG_FULLSCREEN);
setContentView(R.layout.loading);
```

注意书写位置要放在**setContentView**之前!!!

##2.AndroidMenifest.xml文件中

找到要设置的Activity声明标签，设置它的theme即可，例如：

```
		<activity 
            android:name=".MainActivity"
			android:theme=" @android :style/Theme.NoTitleBar.Fullscreen"> 
            <intent-filter> 
                <action android:name="android.intent.action.MAIN" /> 

                <category android:name="android.intent.category.LAUNCHER" /> 
            </intent-filter> 
        </activity> 
```

如果要设置所有页面都为全屏的话就直接将**theme**属性添加到***application***标签中即可，例如：

```
<application 
        android:icon="@drawable/ic_launcher" 
        android:label="@string/app_name" 
        android:theme=" @android :style/Theme.NoTitleBar.Fullscreen"> 
	...
```

Thanks all.

2017.7.30 Beijing 晴

