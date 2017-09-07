#Android手把手撸一个CustomProgressDialog

##1.自定义LoadingView

```
/**
 * Created by xpf on 2017/8/28 :)
 * Function:自定义加载中的Dialog
 */

public class LoadingView extends ProgressDialog {

    public LoadingView(Context context) {
        super(context);
    }

    public LoadingView(Context context, int theme) {
        super(context, theme);
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        init(getContext());
    }

    private void init(Context context) {
        setCancelable(true);
        setCanceledOnTouchOutside(false);
        setContentView(R.layout.loading_view);//loading的xml文件
        WindowManager.LayoutParams params = getWindow().getAttributes();
        params.width = WindowManager.LayoutParams.WRAP_CONTENT;
        params.height = WindowManager.LayoutParams.WRAP_CONTENT;
        getWindow().setAttributes(params);
    }

    @Override
    public void show() { // 显示Dialog
        super.show();
    }

    @Override
    public void dismiss() { // 关闭Dialog
        super.dismiss();
    }
}
```

##2.LoadingView的布局文件

```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="200dp"
    android:layout_height="200dp"
    android:layout_centerInParent="true"
    android:layout_gravity="center_horizontal"
    android:background="@drawable/shape_dialog_bg"
    android:orientation="vertical">

    <RelativeLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="20dp">

        <ProgressBar
            android:id="@+id/pb_load"
            android:layout_width="130dp"
            android:layout_height="130dp"
            android:layout_centerInParent="true"
            android:indeterminateDrawable="@drawable/progressbar" />
    </RelativeLayout>

    <TextView
        android:id="@+id/tv_load_dialog"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:text="加载中..."
        android:textColor="#9a9b98"
        android:textSize="24sp" />
</LinearLayout>
```

##3.dialog圆角的shape

```
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">
    <corners android:radius="16dp" />
    <solid android:color="#88000000" />
</shape>
```

##4.定义drawable旋转动画

```
<animated-rotate xmlns:android="http://schemas.android.com/apk/res/android"
    android:fromDegrees="0"
    android:pivotX="50%"
    android:pivotY="50%"
    android:toDegrees="720">
    <shape
        android:innerRadiusRatio="3"
        android:shape="ring"
        android:thicknessRatio="15"
        android:useLevel="false">
        <gradient
            android:centerColor="#c6c6c6"
            android:centerY="0.50"
            android:endColor="#c6c6c6"
            android:startColor="#55c6c6c6"
            android:type="sweep"
            android:useLevel="false" />
    </shape>
</animated-rotate>
```

##5.定义一个Style

```
	<style name="CustomDialog" parent="Theme.AppCompat.Dialog">
        <item name="android:backgroundDimEnabled">false</item>
        <item name="android:windowBackground">@android:color/transparent</item>
    </style>
```

##6.代码中使用


```
		if (loading == null) {
			loading = new LoadingView(this, R.style.CustomDialog);
			loading.show(); // 显示
        }
```

取消显示：```loading.dismiss();```

ok~终于完了...