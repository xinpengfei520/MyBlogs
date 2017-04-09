#Android Picasso不缓存图片

问题描述：大多APP都有更换个人头像的功能，楼主在开发中遇到了当更换头像时，头像没有更换过来的问题，我用的是Picasso图片请求框架，而这些加载图片的框架一般都会有三级缓存策略，当我们去加载一张图片的时候就会优先查找本地有没有，当一张图片已经加载过了之后就会缓存到本地，虽然我们更换了头像，但是图片的url没有变，所以再次加载的时候还是原来本地缓存的图片，而不是新的图片，所以我们在加载图片的时候可以设置不进行缓存即可，这样每次修改完头像就加载的是新的头像了...

写法如下：

```Picasso.with(mContext).load("http://" + headpic).memoryPolicy(MemoryPolicy.NO_CACHE).transform(new CircleTransform(mContext)).into(ivIcon);```

其中```.memoryPolicy(MemoryPolicy.NO_CACHE)```这句的意思就是不进行缓存。


最后，感谢参考了下面的文章：

![http://blog.csdn.net/u010983881/article/details/50827273](http://blog.csdn.net/u010983881/article/details/50827273)

thanks all.