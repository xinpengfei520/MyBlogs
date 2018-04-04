
# Android Studio3.1.0升级问题记录

每次升级Android Studio时，一般情况下Gradle版本的也会相应的升级，我之前Android Studio 3.0.1、Gradle 是4.1升级后为：Android Studio 3.1.0、Gradle 为4.4。

升级完成后先是报一堆错（每次都升级都是-。-）

##第一个错如下：

```
The SourceSet 'instrumentTest' is not recognized by the Android Gradle Plugin. Perhaps you misspelled something?
```

翻译过来就是：

```
“SourceSet” 的'instrumentTest' 没有被Android Gradle插件识别。也许你拼错吗?
```

猜想可能Gradle移除了'instrumentTest'关键字，网上搜了一下，博客地址如下：[http://www.it1352.com/139247.html](http://www.it1352.com/139247.html)

新版本Gradle对其做了重命名

即：

	 旧版本 - > 新版本
 
	 instrumentTestCompile - > androidTestCompile 

	 instrumentTest  - > androidTest 

然后查看了自己所有module的Gradle，发现有一个地方用到了 **instrumentTest** ，将其改为 **androidTest** ，然后继续同步Gradle，然后编译，这个错误不报了。

##第二个错误如下：

```
Configuration 'compile' is obsolete and has been replaced with 'implementation'.
It will be removed at the end of 2018
```

翻译过来就是：

```
配置“编译”已经过时，取而代之的是“实现”。它将在2018年底被移除。
```

我们知道，在Android Studio3.0开始的时候就将 **compile** 改为 **implementation** 关键字了，当时，项目中大部分的compile改为了implementation，但是还有一部分没有改过来，但是仍然可以使用，但是现在呢？尼玛直接给编译就报错！这不是强制性的让使用 **implementation** 么？报的错中提示2018年底才移除，这还没到年底呢，就不能用了？

还是乖乖的改吧，由于是我们的依赖的library中使用了compile，相当于是library依赖了一个第三方库，直接改为implementation就相当于屏蔽掉了app对library依赖的第三方库api的访问权限，而我们的app刚好用到了library依赖的第三方库的api，所以直接改为implementation是不行的，要想让app能访问到就将其改为 **api**就可以了，其实这不是一个好的解决办法，最好的解决办法是，library中封装一层app访问library依赖的第三方库api的方法，这样的话app是和library依赖的第三方库是没有任何关系的，这样就降低了app与library依赖的第三方库的耦合性，这样也体现了“第三方库隔离”的原则，如果以后依赖的库变了，你只需要修改library中依赖的库及app调用的方法即可，不用动app里面的代码了~

##第三个错误如下

```
Duplicate zip entry [com/c/a/a/a/a.class == 69.jar:com/ta/utdid2/b/a/a.class]
```

翻译过来如下：

```
重复的类，即类所在的包名和类名都相等
```

一般情况下不会写包名和类名相同的类，我猜测一定是Jar包重了，仔细找了一下，发现两个library中都使用了
阿里的httpdns的jar包，只不过是Jar包的版本不一样，旧版本中其中一个library中使用了 **provided** 编译，所以没有报冲突，现在新版本推荐使用 **compileOnly** 关键字，替换为compileOnly关键字后还是报一样的错，哎呦！这就纳闷了，compileOnly的意思不就是编译时依赖，不打紧包中的意思吗？怎么还冲突？仔细查找了一下，发现其中一个library中多了一个混淆规则的jar包（utdid4all-1.1.5.3_proguard.jar），而另外一个没有，但是旧版本是没有问题的，我猜测可能是编译混淆后发现两个类是一样的所以就报错了...

###解决办法一:

两个library中都添加 **utdid4all-1.1.5.3_proguard.jar** 使用 **compileOnly** 关键字编译；

###解决办法二：

两个library中都不添加 **utdid4all-1.1.5.3_proguard.jar**；

###解决办法三：

在app中添加一个 **utdid4all-1.1.5.3_proguard.jar** 进行编译即可；

至此，以上几个错误都解决完了~

##总结

我们应该理解implementation、api、compileOnly等几个关键字的区别，具体见下图：

![这里写图片描述](https://img-blog.csdn.net/20180404101145997?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

最后附上几个参考链接：

[https://docs.gradle.org/current/userguide/java_library_plugin.html#sec:java_library_configurations_graph](https://docs.gradle.org/current/userguide/java_library_plugin.html#sec:java_library_configurations_graph)

[https://stackoverflow.com/questions/44413952/gradle-implementation-vs-api-configuration](https://stackoverflow.com/questions/44413952/gradle-implementation-vs-api-configuration)