#Android使用原生动画工具类实现平移动画


```
	// 向右边移出
	llVideo.setAnimation(AnimationUtils.makeOutAnimation(this, true));
	// 向右边移入
	llVideo.setAnimation(AnimationUtils.makeInAnimation(this, true));
	 
	llVideo.setVisibility(View.VISIBLE);
	llVideo.setVisibility(View.GONE);
	// 向左边移入
	llVideo.setAnimation(AnimationUtils.makeInAnimation(this, false));
	// 向左边移出
	llVideo.setAnimation(AnimationUtils.makeOutAnimation(this, false));
```
