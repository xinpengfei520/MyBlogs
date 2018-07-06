
楼主在编译一个 module 时，遇到了如下错误一直编译不过去

报错如下：

```
Failed to resolve com.android.support
```

刚开始自己写了一个 module demo 编译、运行都没有问题，然后又新建了一个 module，因为在新建之前看到编译好的 module 中有个 xml 文件，感觉命名不好，就 Refactor -> Rename 了一下，重命名了一下文件，一般在重命名的时候 AS 会弹出提示你是否在调用的地方也进行重命名，结果我就直接一路Refactor了，因为之前也是这样修改的，没有出过问题，然后在编译的时候就出现了上面的错误，然后定位到这一行依赖这里：

```
implementation 'com.android.support:appcompat-v7:27.1.1'
```

我看了一下，毛线，这是系统生成的还能写错?为了不打脸，我还是复制了一行好的比对了一下，没有问题的，我看了一下网络，网络也没有问题啊，这不科学呀，懵逼中...

于是乎，打开百度，输入如上错误，点击搜索，看到了一篇博主的文章：

[https://blog.csdn.net/mhl18820672087/article/details/78385361](https://blog.csdn.net/mhl18820672087/article/details/78385361)

我看了一下，我的构建版本的工具是没有任何问题的，因为其他 module 都能编译过，就这一个 module 编译不通过，任然没有解决我的问题，打开我编译不过的 module 的 build.gradle 文件，细心的我发现了一个问题，看下图标红的地方：

![这里写图片描述](https://img-blog.csdn.net/20180706171937624?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

![这里写图片描述](https://img-blog.csdn.net/20180706171947788?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**test_page** 这尼玛是什么鬼？这不是我刚才重命名的名字么？它竟然给我把这个地方的 test 重命名成了 test_page，真是无语了，第一次见这种错，我都没调用，你给我乱重命名，有点说不过去了哦，胸die!

至此，问题算是解决完了...

我发现新版本的 Android Studio 是越来越好用，但是会频繁的出现一些莫名奇葩的问题，大家遇到过吗？

