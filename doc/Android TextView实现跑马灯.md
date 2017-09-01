#Android TextView实现跑马灯

##一、使用TextView属性来实现

1.布局文件

```
	<TextView
        android:id="@+id/tvNotice"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:drawableLeft="@drawable/ico_tongzhi"
        android:drawablePadding="12dp"
        android:ellipsize="marquee"
        android:gravity="center_vertical"
        android:marqueeRepeatLimit="marquee_forever"
        android:singleLine="true"
        android:text="@string/notice_content"
        android:textColor="@color/white"
        android:textSize="24sp"
        android:visibility="visible" />
```

2.代码中

```
	tvContent.setText("xxxx...");
	tvContent.setFocusable(true); // 获取焦点
	tvContent.setFocusableInTouchMode(true);
	tvContent.requestFocus();
```

##二、使用自定义View实现

1.继承TextView并重写isFocused方法，返回true，让其获取焦点

```
/**
 * Created by xpf on 2016/11/19 :)
 * Function:自定义Marquee textView
 */

public class MyTextView extends TextView {

    public MyTextView(Context context) {
        super(context);
    }

    public MyTextView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    public MyTextView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }

    @Override
    public boolean isFocused() {
        return true;
    }
}
```

2.布局文件中使用

```
	<com.anloq.ui.MyTextView
        android:id="@+id/tvNotice"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:drawableLeft="@drawable/ico_tongzhi"
        android:drawablePadding="12dp"
        android:ellipsize="marquee"
        android:gravity="center_vertical"
		android:scrollHorizontally=“true”
        android:marqueeRepeatLimit="marquee_forever"
        android:singleLine="true"
        android:text="@string/notice_content"
        android:textColor="@color/white"
        android:textSize="24sp"
        android:visibility="visible" />
```

3.属性介绍

**marqueeRepeatLimit** 表示跑马灯循环的次数，marquee_forever表示一直循环，也可以写个数字表示次数
然后Activity中可以使设置其settext就ok了；  
**scrollHorizontally** 表示滚动的方向是否为水平方向；

##三、总结

关于这两种实现的的方式评价：

第一种方式采用原生控件来实现，但是存在要在Activity中要获取焦点，我们知道Activity中当前只有一个View控件能获取焦点，这样存在的问题是如果我们的页面中有一个默认要获取焦点的EditText控件时，这样就有问题了，但是方法二就不存在这样的问题，因此，个人偏向于第二种实现的方式，更加灵活，后续也可增加速度等实现个性化的定制。

最后感谢参考文章：

http://www.cnblogs.com/over140/p/3687952.html



