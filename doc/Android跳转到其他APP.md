
#Android跳转到其他APP

从APP A 中打开B APP ，注意 B APP的包名和MainActivity的全类名

```
	Intent intent = new Intent(Intent.ACTION_MAIN);
	intent.addCategory(Intent.CATEGORY_LAUNCHER);
	ComponentName cn = new ComponentName("com.xpf.keyboardtest", "com.xpf.keyboardtest.MainActivity");
	intent.setComponent(cn);
	startActivity(intent);
```