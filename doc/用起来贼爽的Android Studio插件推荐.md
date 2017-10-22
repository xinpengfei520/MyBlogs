
#用起来贼爽的Android Studio插件推荐

自从从Eclipse转到Android Studio之后，就越来越感觉用的越来越喜欢越来越爽，因为Android Studio支持很多很强大功能的各种插件，除此之外还有Android Studio本身的一些强大功能，而Eclipse是与之不能媲美的，今天的主题是主要给大家介绍一些好用、常用、实用的一些Android Studio插件，接下来我就给大家挨个介绍一下把。

##1.Android ButterKnife Zelezny

用过的同志肯定知道使用的酸爽了吧，配合ButterKnife实现注解，自动初始化控件，从此就不用写findViewById了，想着就很爽啊，在Activity，Fragment，Adapter中实现布局xml一键自动生成butterknife注解。

使用步骤：  
1.Android Studio中点击File -> Setting -> Plugin搜索 **Android ButterKnife Zelezny** 点击安装，完成后重启Android Studio;  

![这里写图片描述](http://img.blog.csdn.net/20171017000856835?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlucGVuZ2ZlaTUyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

2.在工程的build.gradle中的**dependencies**节点中添加 **'com.neenbedankt.gradle.plugins:android-apt:1.8'**依赖；  
3.在app的build.gradle中的顶部第二行添加这句**apply plugin: 'com.neenbedankt.android-apt'**；  
4.然后在**dependencies**节点中添加两个依赖库：

```
	compile 'com.jakewharton:butterknife:8.4.0'
    // 添加依赖
    apt 'com.jakewharton:butterknife-compiler:8.4.0'
    // 添加apt
```

5.将鼠标光标放在要生成初始化控件的布局处，然后使用Ctrl+Shift+B选择生成Butterknife注解（每个人的快捷键可能不一样），详情见下图↓

![这里写图片描述](http://img.blog.csdn.net/20171017000958175?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlucGVuZ2ZlaTUyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

生成完后的代码效果如下：

![这里写图片描述](http://img.blog.csdn.net/20171017001041913?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlucGVuZ2ZlaTUyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

##2.GsonFormat

GsonFormat是一个快速将json字符串生成一个JavaBean的插件，免去我们根据json字符串手动写对应JavaBean的过程，而且还支持序列化自动生成。

使用方法：
1.安装插件，和之前的安装方法一样，搜索安装即可；
2.新建一个实体类，然后使用快捷键Alt+Shift+S弹出选择菜单，选择**GsonFormat**,然后会弹出一个窗口，如下图：

![这里写图片描述](http://img.blog.csdn.net/20171017001233599?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlucGVuZ2ZlaTUyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

将json串复制到下图框内：

![这里写图片描述](http://img.blog.csdn.net/20171017001304212?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlucGVuZ2ZlaTUyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

最后点击ok即可，当然我们一般显示点击fromat格式化一下，方便查看，如果有错误或者说少花括号就很容易看出来，json格式错误也会有提示，另外还可以设置使用那种解析json的库比如Gson、Jackson、FastJson等。

##3.ADB WIFI

ADB WIFI是一款使用wifi无线调试你的app的插件，手机无需root权限，就可以很方便地进行无线调试你的应用了，前提是手机和电脑要连接到同一个局域网内。

使用步骤：
1.安装插件，和之前的安装方法一样，搜索安装即可；
2.先用数据线连接到你要调试应用的手机，然后点击Android Studio导航栏的Tools具体操作如下图↓

![这里写图片描述](http://img.blog.csdn.net/20171017001414340?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlucGVuZ2ZlaTUyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

3.拔掉数据线，点击运行，你就会看到你的手机了，然后你就可以尽情的无线调试应用了，是不是很爽？

##4.FindBugs-IDEA

FindBugs-IDEA是一款查找代码中bug的代码审查插件，它可以找出你代码中的一些缺陷和有问题的地方，进而你可以进行代码的优化处理。

使用方法：
1.安装插件，和之前的安装方法一样，搜索安装即可；
2.选中你想要审查的代码的文件名或者包名，然鼠标点击右键，我们可以选择整个包，单个类文件或者真个module去进行分析；

![这里写图片描述](http://img.blog.csdn.net/20171017001503339?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlucGVuZ2ZlaTUyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

3.静静地等一会，分析完之后我们就可以看到有bug的地方就会标注出来，接下来我们就去改这些地方就可以了，当然有些也算不算bug，影响不大我们可以不用改了，但是我们尽量在写代码的时候尽量养成一个良好的编码习惯，这样我们就可以提高编码的质量和效率。

下图就是分析完之后的效果：

![这里写图片描述](http://img.blog.csdn.net/20171017001536053?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlucGVuZ2ZlaTUyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

##5.ETCTranslation

ETCTranslation是一个可以对Android Studio中的英文进行翻译的插件，我们在看源码及英文注释的时候，就不用再去查字典了，可以设置鼠标悬停翻译，也可自定义快捷键，这样方便多了。
下载地址：https://github.com/Skykai521/ECTranslation
安装的时候可以将jar包下载下来导入安装即可。

使用效果如下图：

![这里写图片描述](http://img.blog.csdn.net/20171017001603309?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlucGVuZ2ZlaTUyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

##6.Android Parcelable Code Generator

Android中的序列化有两种方式，分别是实现Serializable接口和Parcelable接口，一般推荐使用Parcelable，只不过我们这种方式要比Serializable方式要繁琐，因此，这个插件就很轻松地帮你实现了。

使用效果如下：

![这里写图片描述](http://img.blog.csdn.net/20171017001622406?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlucGVuZ2ZlaTUyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

##7.SelectorChapek for Android

这是一个非常酷炫的插件，它可以根据一定命名规则的图片来生成出对应空间的选择器的xml，比如点击按钮的选择器以及按下等效果，有了它从此你的生活就可以自理了。

![这里写图片描述](http://img.blog.csdn.net/20171017001656838?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlucGVuZ2ZlaTUyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

##8.AndroidProguardPlugin

AndroidProguardPlugin是一个一键生成项目混淆规则代码的插件，它可以很方便的进行个各种开源库及常用混淆配置，安装方法及下载地址如下：
下载地址：https://github.com/zhonghanwen/AndroidProguardPlugin

使用步骤：
安装完插件之后点击Android Studio导航栏的Edit -> AndroidProGuard即可，弹出对话框后点击ok,如下图：

![这里写图片描述](http://img.blog.csdn.net/20171017001751352?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlucGVuZ2ZlaTUyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

然后打开混淆文件的**proguard-rules.pro**，然后Ctrl + V粘贴即可，最后根据你的项目具体修改文件中报红的地方即可。

##9.Android Postfix completion

Android Postfix completion是一个可以自动根据后缀快速完成代码的插件，它强大的自动预览及补全，从此可以很快速、很愉快的coding了...

插件下载地址：https://plugins.jetbrains.com/plugin/7775?pr=

插件教程地址：http://blog.jetbrains.com/idea/2014/03/postfix-completion/

下图为弹出一个Toast内容为“hello”,你只需输入“hello”.to就会弹出提示，然后选中后回车即可。

![这里写图片描述](http://img.blog.csdn.net/20171017001822078?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlucGVuZ2ZlaTUyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

##10.codota

codota是一个强大的搜索开源库及代码的插件，它搜集了大量的代码，据说有超过700W的代码实例，另外还提供了Chrome和As插件可供下载。

插件下载地址：https://plugins.jetbrains.com/plugin/7638?pr=

插件官方网址：https://www.codota.com/

![这里写图片描述](http://img.blog.csdn.net/20171017001858356?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlucGVuZ2ZlaTUyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

好了，今天的推荐就到这里了，如果你觉得本文对你有帮助，欢迎收藏，欢迎点赞，码字不易，且行且珍惜！

注：本文为作者原创，转载请注明出处，谢谢！