
#<span  style="color:#1e819e;">Android实现多语言so easy</span>

>微信公众号：CodingAndroid  
CSDN：[http://blog.csdn.net/xinpengfei521](http://blog.csdn.net/xinpengfei521)
声明：本文由CodingAndroid原创，未经授权，不可随意转载！

&#160; &#160;&#160; &#160;最近，我们公司的业务已经拓展到了香港，我们都知道香港使用的是繁体中文，因此，我们的APP要可以设置繁体语言，这不我们要紧跟国际的步伐，实现多语言，产品定给我们的需求主要以实现简体中文、繁体中文、英文三种语言切换即可，具体的业务逻辑是：当用户第一次进入APP时，App的语言跟随当前系统语言，当用户设置了某种语言之后就切换为用户设置的语言，不管系统之后设置成哪种语言，都不会影响用户设置的语言，如果用户一直没有设置语言选项，只要系统语言改变时，APP的语言也要跟随系统语言设置改变。

&#160; &#160;&#160; &#160;说明：本文以实现简体中文、繁体中文和英语为例进行简要讲解。

##1.实现思路

&#160; &#160;&#160; &#160;我们可以预先使用SharedPreference来保存一个语言类型的值，当用户第一次进入APP时，我们通过Key取出这个值，第一次肯定是取不到的，这时我们将App的语言设置为当前系统默认值即可；当用户通设置多语言时，我们将这个语言对应的值通过SharedPreference保存到本地即可，然后此时重启APP，设置为用户设置的语言即可，以后每次进入App时只需取出保存用户设置的语言设置即可，一般设置语言写在程序的入口 **Application** 的 **onCreate()** 方法里。

##2.自定义SharedPreference工具类

&#160; &#160;&#160; &#160;作用：用于保存当前设置语言的类型，此文以SharedPreference保存为例，当然使用数据库或者其他保存方式也是可以的。

```
/**
 * Created by xpf on 2017/03/25 :)
 * Function: sp存储的工具类
 */
public class SpUtil {

    private static final String APP_SP = "app_sp";
    private static final String TAG = SpUtil.class.getSimpleName();

    private SpUtil() {
    }

    private static SpUtil instance = new SpUtil();
    private static SharedPreferences mSp = null;

    public static SpUtil getInstance() {
        if (mSp == null) {
            mSp = MyApplication.getContext().getSharedPreferences(APP_SP, Context.MODE_PRIVATE);
        }
        return instance;
    }

    /**
     * 保存数据
     *
     * @param key   键
     * @param value 值
     */
    public void save(String key, Object value) {
        if (value == null) {
            Log.e(TAG, "value==null保存失败");
            return;
        }
        if (value instanceof String) {
            mSp.edit().putString(key, (String) value).commit();
        } else if (value instanceof Boolean) {
            mSp.edit().putBoolean(key, (Boolean) value).commit();
        } else if (value instanceof Integer) {
            mSp.edit().putInt(key, (Integer) value).commit();
        }
    }

    /**
     * 读取String类型数据
     *
     * @param key
     * @param defValue
     * @return
     */
    public String getString(String key, String defValue) {
        return mSp.getString(key, defValue);
    }

    /**
     * 读取boolean类型数据
     *
     * @param key
     * @param defValue
     * @return
     */
    public boolean getBoolean(String key, boolean defValue) {
        return mSp.getBoolean(key, defValue);
    }

    /**
     * 读取boolean类型数据
     *
     * @param key
     * @param defValue
     * @return
     */
    public int getInt(String key, int defValue) {
        return mSp.getInt(key, defValue);
    }

    /**
     * 清除所有保存的数据(xxx.xml仍然存在，但是内部没有数据)
     */
    public void clearAll() {
        mSp.edit().clear().commit();
    }
}
```

##3.自定义LocaleUtil工具类

&#160; &#160;&#160; &#160;作用：用于设置保存语言及获取当前语言，重启APP等操作

```
public class LocaleUtil {

    /**
     * 获取用户设置的Locale
     *
     * @return Locale
     */
    public static Locale getUserLocale() {
        int currentLanguage = SpUtil.getInstance().getInt("currentLanguage", 0);
        Locale myLocale = Locale.SIMPLIFIED_CHINESE;
        switch (currentLanguage) {
            case 0:
                myLocale = Locale.SIMPLIFIED_CHINESE;
                break;
            case 1:
                myLocale = Locale.ENGLISH;
                break;
            case 2:
                myLocale = Locale.TRADITIONAL_CHINESE;
                break;
        }
        return myLocale;
    }

    /**
     * 设置语言：如果之前有设置就遵循设置如果没设置过就跟随系统语言
     */
    public static void changeAppLanguage(Context context) {
        if (context == null) return;
        Context appContext = context.getApplicationContext();
        int currentLanguage = SpUtil.getInstance().getInt("currentLanguage", -1);
        Locale myLocale;
        // 0 简体中文 1 繁体中文 2 English
        switch (currentLanguage) {
            case 0:
                myLocale = Locale.SIMPLIFIED_CHINESE;
                break;
            case 1:
                myLocale = Locale.TRADITIONAL_CHINESE;
                break;
            case 2:
                myLocale = Locale.ENGLISH;
                break;
            default:
                myLocale = appContext.getResources().getConfiguration().locale;
        }
        // 本地语言设置
        if (needUpdateLocale(appContext, myLocale)) {
            updateLocale(appContext, myLocale);
        }
    }

    /**
     * 保存设置的语言
     *
     * @param currentLanguage index
     */
    public static void changeAppLanguage(Context context, int currentLanguage) {
        if (context == null) return;
        Context appContext = context.getApplicationContext();
        SpUtil.getInstance().save("currentLanguage", currentLanguage);
        Locale myLocale = Locale.SIMPLIFIED_CHINESE;
        // 0 简体中文 1 繁体中文 2 English
        switch (currentLanguage) {
            case 0:
                myLocale = Locale.SIMPLIFIED_CHINESE;
                break;
            case 1:
                myLocale = Locale.TRADITIONAL_CHINESE;
                break;
            case 2:
                myLocale = Locale.ENGLISH;
                break;
        }
        // 本地语言设置
        if (LocaleUtil.needUpdateLocale(appContext, myLocale)) {
            LocaleUtil.updateLocale(appContext, myLocale);
        }

        Toast.makeText(appContext, appContext.getString(R.string.set_success), Toast.LENGTH_SHORT).show();
        restartApp(appContext);
    }

    /**
     * 重启app生效
     *
     * @param context
     */
    public static void restartApp(Context context) {
        Intent intent = new Intent(context, MainActivity.class);
        intent.setAction(Intent.ACTION_MAIN);
        intent.addCategory(Intent.CATEGORY_LAUNCHER);
        intent.setFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK | Intent.FLAG_ACTIVITY_NEW_TASK);
        context.startActivity(intent);
    }

    /**
     * 获取当前的Locale
     *
     * @param context Context
     * @return Locale
     */
    public static Locale getCurrentLocale(Context context) {
        Locale locale;
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) { //7.0有多语言设置获取顶部的语言
            locale = context.getResources().getConfiguration().getLocales().get(0);
        } else {
            locale = context.getResources().getConfiguration().locale;
        }
        return locale;
    }

    /**
     * 更新Locale
     *
     * @param context Context
     * @param locale  New User Locale
     */
    public static void updateLocale(Context context, Locale locale) {
        if (needUpdateLocale(context, locale)) {
            Configuration configuration = context.getResources().getConfiguration();
            if (Build.VERSION.SDK_INT >= 19) {
                configuration.setLocale(locale);
            } else {
                configuration.locale = locale;
            }
            DisplayMetrics displayMetrics = context.getResources().getDisplayMetrics();
            context.getResources().updateConfiguration(configuration, displayMetrics);
        }
    }

    /**
     * 判断需不需要更新
     *
     * @param context Context
     * @param locale  New User Locale
     * @return true / false
     */
    public static boolean needUpdateLocale(Context context, Locale locale) {
        return locale != null && !getCurrentLocale(context).equals(locale);
    }

    /**
     * 当系统语言发生改变的时候还是继续遵循用户设置的语言
     *
     * @param context
     * @param newConfig
     */
    public static void setLanguage(Context context, Configuration newConfig) {
        if (context == null) return;
        Context appContext = context.getApplicationContext();
        int currentLanguage = SpUtil.getInstance().getInt("currentLanguage", -1);
        Locale locale;
        // 0 简体中文 1 繁体中文 2 English
        switch (currentLanguage) {
            case 0:
                locale = Locale.SIMPLIFIED_CHINESE;
                break;
            case 1:
                locale = Locale.TRADITIONAL_CHINESE;
                break;
            case 2:
                locale = Locale.ENGLISH;
                break;
            default:
                locale = appContext.getResources().getConfiguration().locale;
        }
        // 系统语言改变了应用保持之前设置的语言
        if (locale != null) {
            Locale.setDefault(locale);
            Configuration configuration = new Configuration(newConfig);
            if (Build.VERSION.SDK_INT >= 19) {
                configuration.setLocale(locale);
            } else {
                configuration.locale = locale;
            }
            appContext.getResources().updateConfiguration(configuration, appContext.getResources().getDisplayMetrics());
        }
    }
}
```

###3.1Application中调用，用于初始化语言设置

```
	@Override
    public void onCreate() {
        super.onCreate();
        LocaleUtil.changeAppLanguage(this);
    }
```

###3.2在Application中重写如下方法：用于当系统设置语言变化时进行语言设置

```
	@Override
    public void onConfigurationChanged(Configuration newConfig) {
        super.onConfigurationChanged(newConfig);
        Log.e("TAG", "onConfigurationChanged");
        LocaleUtil.setLanguage(mContext, newConfig);
    }
```

###3.3在多语言设置界面设置APP语言

&#160; &#160;&#160; &#160;当用户点击保存时传入当前语言对应的Index值即可，我这里使用的是保存一个Int类型的值，每一个Int类型的值对应一种语言的类型，当然你可以根据自己的实现保存为String类型也是可以的。

```
LocaleUtil.changeAppLanguage(mContext, currentLanguage);
```

##4.多国语言文件夹命名对应表如下：

&#160; &#160;&#160; &#160;本文只以简体中文、繁体中文和英语为例，要想实现更多语言设置，请参考下表：

|国家|Folder Name|  
| ------ | ------ |  
|中文（中国）|values-zh-rCN|  
|中文（台湾）|values-zh-rTW|  
|中文（香港）|values-zh-rHK|  
|英语（美国）|values-en-rUS|  
|英语（英国）|values-en-rGB|  
|英文（澳大利亚）|values-en-rAU|  
|英文（加拿大）|values-en-rCA|  
|英文（爱尔兰）|values-en-rIE|  
|英文（印度）|values-en-rIN|  
|英文（新西兰）|values-en-rNZ|  
|英文（新加坡）|values-en-rSG|  
|英文（南非）|values-en-rZA|  
|阿拉伯文（埃及）|values-ar-rEG|  
|阿拉伯文（以色列）|values-ar-rIL|  
|保加利亚文|values-bg-rBG|  
|加泰罗尼亚文|values-ca-rES|  
|捷克文|values-cs-rCZ|  
|丹麦文|values-da-rDK|  
|德文（奥地利）|values-de-rAT|  
|德文（瑞士）|values-de-rCH|  
|德文（德国）|values-de-rDE|  
|德文（列支敦士登）|values-de-rLI|  
|希腊文|values-el-rGR|  
|西班牙文（西班牙）|values-es-rES|  
|西班牙文（美国）|values-es-rUS|  
|芬兰文（芬兰）|values-fi-rFI|  
|法文（比利时）|values-fr-rBE|  
|法文（加拿大）|values-fr-rCA|  
|法文（瑞士）|values-fr-rCH|  
|法文（法国）|values-fr-rFR|  
|希伯来文|values-iw-rIL|  
|印地文|values-hi-rIN|  
|克罗里亚文|values-hr-rHR|  
|匈牙利文|values-hu-rHU|  
|印度尼西亚文|values-in-rID|  
|意大利文（瑞士）|values-it-rCH|  
|意大利文（意大利）|values-it-rIT|  
|日文|values-ja-rJP|  
|韩文|values-ko-rKR|  
|立陶宛文|valueslt-rLT|  
|拉脱维亚文|values-lv-rLV|  
|挪威博克马尔文|values-nb-rNO|  
|荷兰文(比利时)|values-nl-BE|  
|荷兰文（荷兰）|values-nl-rNL|  
|波兰文|values-pl-rPL|  
|葡萄牙文（巴西）|values-pt-rBR|  
|葡萄牙文（葡萄牙）|values-pt-rPT|  
|罗马尼亚文|values-ro-rRO|  
|俄文|values-ru-rRU|  
|斯洛伐克文|values-sk-rSK|  
|斯洛文尼亚文|values-sl-rSI|  
|塞尔维亚文|values-sr-rRS|  
|瑞典文|values-sv-rSE|  
|泰文|values-th-rTH|  
|塔加洛语|values-tl-rPH|  
|土耳其文|values--r-rTR|  
|乌克兰文|values-uk-rUA|  
|越南文|values-vi-rVN|  


##5.常见的一些坑

 - 设置英文语言时在有些手机上不起作用
   在开发过程中我们试过在Redmi Note 4手机上设置英语语言不起作用，这是由于有些手机厂商的默认英语设置的是英式英语，而我们只使用了美式英语的缘故，此时处理方式为：①设置两种英语的设置选项，即美式英语和英式英语；②可将res的文件夹命名为**values-en**即可，不区分英式英语和美式英语即可；
 - 在Android Studio3.0之前，如果有些strings没有对应到其他的语种，即有些字段没有翻译过来，打包时会       报警告，可以打成包，而3.0之后是不可以的，会打包失败，所以每个Strings必须对应一套完整的翻译！

##6.相关权限

```
<uses-permission android:name="android.permission.CHANGE_CONFIGURATION"/>
```

##7.更多探讨

&#160; &#160;&#160; &#160;在实现APP种切换语言时，试了试微信的多语言设置页面是在4级页面，当它设置完语言之后先是跳转到**我** 界面（一级页面），然后紧接着跳到设置界面（二级页面），貌似微信没有重启App，它只是管理保存了Activity任务栈，最后跳转到设置界面，而我的例子只是重启了App没有跳转回到设置界面，当然这要看具体的需求了，我个人感觉，设置完就没有必要再跳转回去了。

***

最后附上Demo:
[https://github.com/xinpengfei520/Multi-Language](https://github.com/xinpengfei520/Multi-Language)

如果你觉得不错可以帮我点个star,3Q~

若在使用过程中遇到什么问题，或有好提议，欢迎在公众号“CodingAndroid”中提出
![这里写图片描述](http://img.blog.csdn.net/20170911223615073)