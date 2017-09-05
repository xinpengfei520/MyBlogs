#Android实现帧动画

##1.在res的drawable下新建一个布局**animation.xml***文件

```<?xml version="1.0" encoding="utf-8"?>
<animation-list xmlns:android="http://schemas.android.com/apk/res/android"
    android:oneshot="true">

    <item
        android:drawable="@drawable/image1"
        android:duration="32" />
    <item
        android:drawable="@drawable/image2"
        android:duration="32" />
    <item
        android:drawable="@drawable/image3"
        android:duration="32" />
    <item
        android:drawable="@drawable/image4"
        android:duration="32" />
    <item
        android:drawable="@drawable/image5"
        android:duration="32" />
    <item
        android:drawable="@drawable/image6"
        android:duration="32" />
    <item
        android:drawable="@drawable/image7"
        android:duration="32" />
    <item
        android:drawable="@drawable/image8"
        android:duration="32" />
    <item
        android:drawable="@drawable/image9"
        android:duration="32" />
    <item
        android:drawable="@drawable/image10"
        android:duration="32" />
    <item
        android:drawable="@drawable/image11"
        android:duration="32" />
    <item
        android:drawable="@drawable/image12"
        android:duration="32" />
    <item
        android:drawable="@drawable/image13"
        android:duration="32" />
    <item
        android:drawable="@drawable/image14"
        android:duration="32" />
    <item
        android:drawable="@drawable/image15"
        android:duration="32" />
    <item
        android:drawable="@drawable/image16"
        android:duration="32" />
    <item
        android:drawable="@drawable/image17"
        android:duration="32" />
    <item
        android:drawable="@drawable/image18"
        android:duration="32" />
    <item
        android:drawable="@drawable/image19"
        android:duration="32" />
    <item
        android:drawable="@drawable/image20"
        android:duration="32" />
    <item
        android:drawable="@drawable/image21"
        android:duration="32" />
    <item
        android:drawable="@drawable/image22"
        android:duration="32" />
    <item
        android:drawable="@drawable/image23"
        android:duration="32" />
    <item
        android:drawable="@drawable/image24"
        android:duration="32" />
    <item
        android:drawable="@drawable/image25"
        android:duration="32" />
    <item
        android:drawable="@drawable/image26"
        android:duration="32" />
    <item
        android:drawable="@drawable/image27"
        android:duration="32" />
    <item
        android:drawable="@drawable/image28"
        android:duration="32" />
    <item
        android:drawable="@drawable/image29"
        android:duration="32" />
    <item
        android:drawable="@drawable/image30"
        android:duration="32" />
    <item
        android:drawable="@drawable/image31"
        android:duration="32" />
    <item
        android:drawable="@drawable/image32"
        android:duration="32" />
    <item
        android:drawable="@drawable/image33"
        android:duration="32" />
    <item
        android:drawable="@drawable/image34"
        android:duration="32" />
    <item
        android:drawable="@drawable/image35"
        android:duration="32" />
    <item
        android:drawable="@drawable/image36"
        android:duration="32" />
    <item
        android:drawable="@drawable/image37"
        android:duration="32" />
    <item
        android:drawable="@drawable/image38"
        android:duration="32" />
    <item
        android:drawable="@drawable/image39"
        android:duration="32" />
    <item
        android:drawable="@drawable/image40"
        android:duration="32" />
    <item
        android:drawable="@drawable/image41"
        android:duration="32" />
    <item
        android:drawable="@drawable/image42"
        android:duration="32" />
    <item
        android:drawable="@drawable/image43"
        android:duration="32" />
    <item
        android:drawable="@drawable/image44"
        android:duration="32" />
    <item
        android:drawable="@drawable/image45"
        android:duration="32" />
    <item
        android:drawable="@drawable/image46"
        android:duration="32" />
    <item
        android:drawable="@drawable/image47"
        android:duration="32" />
    <item
        android:drawable="@drawable/image48"
        android:duration="32" />
    <item
        android:drawable="@drawable/image49"
        android:duration="32" />
    <item
        android:drawable="@drawable/image50"
        android:duration="32" />
    <item
        android:drawable="@drawable/image51"
        android:duration="32" />
    <item
        android:drawable="@drawable/image52"
        android:duration="32" />
    <item
        android:drawable="@drawable/image53"
        android:duration="32" />
    <item
        android:drawable="@drawable/image54"
        android:duration="32" />
    <item
        android:drawable="@drawable/image55"
        android:duration="32" />
    <item
        android:drawable="@drawable/image56"
        android:duration="32" />
    <item
        android:drawable="@drawable/image57"
        android:duration="32" />
    <item
        android:drawable="@drawable/image58"
        android:duration="32" />
    <item
        android:drawable="@drawable/image59"
        android:duration="32" />
    <item
        android:drawable="@drawable/image60"
        android:duration="32" />

</animation-list>
```

**android:oneshot="true"**当为true时表示只播放一次，为false时表示循环播放；

##2.给ImageView设置帧动画的src

```
	<ImageView
        android:id="@+id/ivLoading"
        android:layout_width="300dp"
        android:layout_height="300dp"
        android:layout_centerInParent="true"
        android:src="@drawable/animation"
        android:visibility="gone" />
```

##3.播放帧动画

```
	ivLoading.setImageResource(R.drawable.animation);
	drawable = (AnimationDrawable) ivLoading.getDrawable();
	drawable.start();
```

##4.以上播放默认是没有监听的，所以我们这里提供一种自定义View，可以监听帧动画播完的监听

```
/**
 * 自定义可以播放动态图片的ImageView
 */
public class AnimationImageView extends ImageView {

	public AnimationImageView(Context context) {
		super(context);
	}

	public AnimationImageView(Context context, AttributeSet attrs) {
		super(context, attrs);
	}

	public AnimationImageView(Context context, AttributeSet attrs, int defStyle) {
		super(context, attrs, defStyle);
	}

	
	
	public interface OnFrameAnimationListener{
		/**
		 * 动画开始播放后调用
		 */
		void onStart();
		/**
		 * 动画结束播放后调用
		 */
		void onEnd();
	}

	/**
	 * 不带动画监听的播放
	 * @param resId
	 */
	public void loadAnimation(int resId){
		setImageResource(resId);
		AnimationDrawable anim = (AnimationDrawable)getDrawable();
		anim.start();
	}
	
	/**
	 * 带动画监听的播放
	 * @param resId
	 * @param listener
	 */
	public void loadAnimation(int resId, final OnFrameAnimationListener listener) {
		setImageResource(resId);
		AnimationDrawable anim = (AnimationDrawable)getDrawable();
		anim.start();
		if(listener != null){
			// 调用回调函数onStart
			listener.onStart();
		}
		
		// 计算动态图片所花费的事件
		int durationTime = 0;
        for (int i = 0; i < anim.getNumberOfFrames(); i++) {
            durationTime += anim.getDuration(i);
        }
        
        // 动画结束后
        new Handler().postDelayed(new Runnable() {
			
			@Override
			public void run() {
				if(listener != null){
					// 调用回调函数onEnd
					listener.onEnd();
				}
			}
		}, durationTime);
	}
}
```

##5.代码中加载并设置监听

```
			// 加载带监听的帧动画
			anim_view.loadAnimation(R.drawable.anim_defense,
					new OnFrameAnimationListener() {

						@Override
						public void onStart() {
							// 动画刚播放时
						}

						@Override
						public void onEnd() {
							// 动画结束播放时
						}
					});
```

参考文章：

http://blog.csdn.net/zuozuoshenghen/article/details/50249569

http://blog.csdn.net/u012975705/article/details/48717391

http://www.cnblogs.com/net168/p/4204797.html