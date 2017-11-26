#Android Studio曲折更新之路

Android studio 3.0版本出来也好长时间了，自己的电脑每次有更新我就立马回更新成最新的，公司的电脑自从装上AS就没有更新过，主要是工作比较忙没有时间去更新，这不周末把电脑带回来想着更新成最新的版本...

##1.开始更新

打开AS 点击Help --> Check for Updates...

等了一小会后弹出的是**Download**而不是**Update and Restart**安装更新并重启的按钮，点击**Download**按钮之后，跳转到浏览器，让下载新版本，纳尼？这是什么鬼？我自己的电脑每次更新都是自己下载安装完补丁之后自动安装并重启的，下载完再安装也是可以的，就是比较麻烦而已，而我不想使用这种方式去安装，最后搜了一下大概的意思就是Android Studio的版本太低了不支持使用更新补丁的方式升级到最新的版本，我现在的版本是2.2.3想升级到最新的3.0.1版本，而Google不提供补丁包，可能是版本跨度太大了吧，我想着肯定是有办法的，最后在CSDN上找到一篇博客，和我的情况一样，使用手动更新Android Studio到最新版本，按照他的办法步骤如下：

###1.1找到最新的版本号对应的串号

https://dl.google.com/Android/studio/patches/updates.xml

如下图所示：

###1.2下载你对应版本的到最新版本的补丁包

点开AS的Setting->在点开Update就可以看到当前版本的串号，如下图：

然后拼接出一个下载补丁包的url:如下

```https://dl.google.com/android/studio/patches/AI-145.3537739-162.4069837-patch-win.jar```

这样就可以下载对应的版本的补丁包，我第一次拼接时，没有自动下载，然后响应超时了，估计是版本太旧的缘故，刚才在https://dl.google.com/Android/studio/patches/updates.xml网页中看到了许多版本更新到最新版本的更新包，仔细看了看，确实没有我这个版本的更新补丁包，找到最低的版本的补丁包是2.3.3，也就是2.3.3肯定是可以更新到3.0.1的，所有有个折中的办法是先更新到2.3.3，再更新到3.0.1，于是从上面的网页中找到2.3.3对应的版本串口，拼接出一个下载的url,复制到浏览器中，点击回车... WC！！！FUCK!居然可以下载了，太爽了，马上就下载完成了。

###1.3手动安装AS补丁包

打开CMD，切换到AS安装的更目录，输入一下命令后回车：

```java -classpath E:\AI-145.3537739-162.4069837-patch-win.jar com.intellij.updater.Runner install .```

命令格式格式：java -classpath + 更新包的路径 + com.intellij.updater.Runner install .

稍等一会更新包就安装完成了

然后打开AS，会提示AS已经更新，提示你是否导入之前版本的设置，说明AS已经更新，进去之后再查看版本已经更新到2.3.3了，至此更新的第一步已经完成，接下来就简单了，点击Check for Updates...弹出窗口中终于出现了**Update and Restart**按钮，点击该按钮即可自动下载并更新...

最后附上2.2.3-2.3.3的更新包，需要的可自行下载。

http://download.csdn.net/download/xinpengfei521/10133487

另外，感谢下面博主的博客：

http://blog.csdn.net/u010798604/article/details/52838030