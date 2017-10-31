
#ListView设置分割线

##1.设置分割的颜色和高度

**divider**设置分割线的颜色属性；

**dividerHeight**设置分割线的高度；

```
	<ListView
        android:id="@+id/listView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginLeft="@dimen/margin_14dp"
        android:layout_marginRight="@dimen/margin_14dp"
        android:layout_marginTop="@dimen/margin_8dp"
        android:divider="@color/transparent"
        android:dividerHeight="8dp" />
```

##2.去掉分割线

1.在代码中设置：setDriver(null)；

2.或者在布局文件中设置：android:divider="@null"；

3.如果没有背景也可将divider设置为透明也可以实现隐藏分割线的效果，但是这种方式分割线还是存在的，并且占了一定的高度，只是隐藏显示了而已；

更多关于**ListView**的文章请参考：

http://blog.csdn.net/bdmh/article/details/39522071

http://blog.csdn.net/fly_yuge/article/details/52473978

http://blog.csdn.net/lovexieyuan520/article/details/50846569

http://blog.csdn.net/zhangphil/article/details/48948217

http://www.cnblogs.com/SZ2015/p/4558462.html

http://flycatdeng.iteye.com/blog/2273790

http://www.cnblogs.com/zhujiabin/p/5818091.html

http://blog.csdn.net/world_kun/article/details/51545234