#Multiple dex files define Lokhttp3/internal/wsWebSocketProtocol

##老套路，先晒图

图一：
![这里写图片描述](http://img.blog.csdn.net/20180122114830989?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlucGVuZ2ZlaTUyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
如题，在编译打包时遇到了如上错误，很明显这是一个依赖库冲突的问题，我的app下依赖一个 **ad** library,而 **ad** library有依赖了一个 **http** library,这是一个我用okhttp封装的网络请求库，而 **ad** library 又依赖了 **compile 'com.squareup.okhttp3:okhttp:3.9.1'** 这依赖关系确实有点混乱，那这时候有人说了，你的 **ad** library为什么不用 **http** library里面的okhttp呢？自从AS3.0 gradle4.0开始依赖使用 **implementation** 代替了 **compile** 关键字，因此 **compile** 就被过期了，**http** 虽然依赖了 **okhttp** ，但是 **ad** library要想使用 **okhttp** 是不可以的，因为使用**implementation** 对于**ad** library 是不可见的，所以**ad** library又依赖了一次 **compile 'com.squareup.okhttp3:okhttp:3.9.1'** 因为 **ad** library需要单独的进行定制化的网络请求配置，所以 **ad** library中的网络请求是没有封装进**http** library中的，所以导致了重复依赖的问题，使用  **implementation** 关键字虽然可以屏蔽**ad** library访问 **http** library中 **okhttp** 的api,但是依赖冲突的问题确实还是存在的...

于是乎，网上搜了一篇帖子：

https://stackoverflow.com/questions/45111497/android-gradle-failed-multiple-dex-files-define-okhttp

看来一下和我的情况一毛一样，报的错误也一样，直接戳到Answer处，解决方式如下

图二：
![这里写图片描述](http://img.blog.csdn.net/20180122114849455?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlucGVuZ2ZlaTUyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

然后直接去除掉重复的依赖，修改如下后，再rebuild

图三：
![这里写图片描述](http://img.blog.csdn.net/20180122114902536?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlucGVuZ2ZlaTUyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

心里想着，这个问题终于解决了，喝了口水，长呼了一口气。。。

我擦，竟然又爆红了，我勒个去，什么情况？这不科学啊，什么鬼？心里很郁闷，这是为什么呢？

不行啊，看来我必须要放大招了，心里开始仔细揣摩了，认真想想，绝逼哪里肯定是有问题的，start thinking...

认真起来的男人是最帅的 0.0~

仔细看了看**http** library中的 **okhttp** 是3.3.1,而 **ad** library中的 **okhttp** 3.9.1,忽然想起来之前遇到过同时依赖两个不同版本的库也会出现这样的问题，于是，灵光一现，立马把版本号改一样试试，修改后如下:

图四：
![这里写图片描述](http://img.blog.csdn.net/20180122114929435?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlucGVuZ2ZlaTUyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

然后再rebuild...

oh yeah ~ 搞定，就是这么6... 怎一个爽快了得？

好了，今天就分享这么多吧，一来呢总结一下自己遇到的一些问题及解决办法，如果你也遇到，希望能帮助到你们，二来呢，希望大家如果真的帮助到你们了，麻烦你们给我点个赞或者在文章底部给我评论，或者你有什么意见或者意见都可以相互交流，谢谢你们！

    --------一个逗比的码农和一个逗比的故事，如果想看到更多的文章，请点左侧头像下面的关注！

