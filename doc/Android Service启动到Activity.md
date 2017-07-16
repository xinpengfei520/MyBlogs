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

##3.为什么Android在Service 里面启动Activity要强制规定使用参数FLAG_ACTIVITY_NEW_TASK呢？

我们可以来做这样一个假设，我们有这样一个需求：
我们在电话本里面启动一个Service，然后它执行5分钟后，启动一个Activity
那么很有可能用户在5分钟后已经不在电话本程序里面操作了，有可能去上网，打开浏览器程序了。
5分钟后，此时当前的Task是浏览器的task，那么弹出Activity，如果这个Activity在当前Task的话，也就是浏览器的Task；那么用户就会觉得莫名其妙；因为弹出的Activity和浏览器在一个Task，本来这个Activity应该属于电话本的。
所以，对于Service而言，干脆强制定义启动的Activity要创建一个新的Task.
这种设计，我觉得还是比较合理的。

最后感谢参考文章：![http://bbs.51cto.com/thread-1133875-1.html](http://bbs.51cto.com/thread-1133875-1.html)

Thanks all.