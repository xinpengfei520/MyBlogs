
# Expected resource of type id [ResourceType]

今天刚升级完Android Studio到3.0.1版本，装完之后，遇到一堆问题，改了一大堆问题，终于改完了，就想打包apk试试，没想到打包时报了如下错误：

```Error Expected resource of type id [ResourceType]```

点击到报错的地方，原来是依赖的一个开源库一个**if**判断处报错了，如下图：

但是点击进去之后看了一下也是返回的**Int**类型的值，我就郁闷了，为什么不能比较了？有哪位大神可以解释一下是为什么吗？

解决办法：

后来在网上搜了一下，只要在这个报错代码所在方法的头上加入如下代码即可：

```@SuppressWarnings("ResourceType")```

试了试，果然好使，但还是不知道为什么？

大神求解释！！！