# Android 设置动画变化的速率

我们知道 Android 的动画有视图动画，属性动画，帧动画等等，对于视图动画我们是可以设置动画的变化速率的，有加速，减速，先加速再减速...等等各种变化速率的效果。对于视图动画，创建动画有两种方式，一种是在 xml 文件中创建，另一种是在代码中创建，因此对应的设置动画变化速率也有两种方式。

## 一、xml 中设置动画的变化速率

1.设置动画为加速动画(动画播放中越来越快)：

```
android:interpolator="@android:anim/accelerate_interpolator"
```

2.设置动画为减速动画(动画播放中越来越慢) :

```
android:interpolator="@android:anim/decelerate_interpolator"
```

3.设置动画为先加速在减速(开始速度最快 逐渐减慢)：

```
android:interpolator="@android:anim/accelerate_decelerate_interpolator"
```

4.先反向执行一段，然后再加速反向回来（相当于我们弹簧，先反向压缩一小段，然后在加速弹出）：

```
android:interpolator="@android:anim/anticipate_interpolator"
```

5.同上先反向一段，然后加速反向回来，执行完毕自带回弹效果（更形象的弹簧效果）:

```
android:interpolator="@android:anim/anticipate_overshoot_interpolator"
```

6.执行完毕之后会回弹跳跃几段（相当于我们高空掉下一颗皮球，到地面是会跳动几下）:

``` 
android:interpolator="@android:anim/bounce_interpolator"
```

7.循环，动画循环一定次数，值的改变为一正弦函数：Math.sin(2* mCycles* Math.PI* input):

```
android:interpolator="@android:anim/cycle_interpolator"
```

8.线性均匀改变:

```
android:interpolator="@android:anim/linear_interpolator"
```

9.加速执行，结束之后回弹:

```  
android:interpolator="@android:anim/overshoot_interpolator"
```

## 二、代码中设置动画的变化速率

1.设置动画为加速动画(动画播放中越来越快)：

```
animation.setInterpolator(new AccelerateInterpolator());
```

2.设置动画为减速动画(动画播放中越来越慢) :

```
animation.setInterpolator(new DecelerateInterpolator());
```

3.设置动画为先加速在减速(开始速度最快 逐渐减慢)：

```
animation.setInterpolator(new AccelerateDecelerateInterpolator());
```

4.先反向执行一段，然后再加速反向回来（相当于我们弹簧，先反向压缩一小段，然后在加速弹出）:

```
animation.setInterpolator(new AnticipateInterpolator());
```

5.同上先反向一段，然后加速反向回来，执行完毕自带回弹效果（更形象的弹簧效果）:

```
animation.setInterpolator(new AnticipateOvershootInterpolator());
```

6.执行完毕之后会回弹跳跃几段（相当于我们高空掉下一颗皮球，到地面是会跳动几下）:

```
animation.setInterpolator(new BounceInterpolator());
```

7.循环，动画循环一定次数，值的改变为一正弦函数：Math.sin(2* mCycles* Math.PI* input):

```
animation.setInterpolator(new CycleInterpolator(2));
```

8.线性均匀改变:

```
animation.setInterpolator(new LinearInterpolator());
```

9.加速执行，结束之后回弹:

```
animation.setInterpolator(new OvershootInterpolator());
```

如果觉得文章不错或者有帮到你的话，欢迎大家点赞和评论！

**你还可以在下面这些地方也可以找到我：**

>微信公众号:「IT大飞说」
GitHub:https://github.com/xinpengfei520
CSDN:https://blog.csdn.net/xinpengfei521
简书：https://www.jianshu.com/u/268aa8e1a12f
掘金：https://juejin.im/user/57c82d0779bc440063eac0f8
CnBlog:https://www.cnblogs.com/angel88/
知乎：https://www.zhihu.com/people/xin-peng-fei-9/activities
微博：https://weibo.com/u/1783451275?is_all=1
Blog:http://www.x-sir.com