#java基本数据类型转换为String

我们都知道把一个基本数据类型转为String类型，一般有三种方式，例如：有一个Integer型数据a，可以使用i.toString()、String.valueOf(i)、i+””三种方式，三种方式的效率如何，接下来我们做一个小测试：

##1.测试代码如下：

```
/**
     * 把一个基本数据类型转为字符串
     */
    private void testExecuteTime() {
        int loopTime = 100000;
        //Integer i = 0;
        Long i = 0L;
        long startTime;

        startTime = System.currentTimeMillis();
        for (int j = 0; j < loopTime; j++) {
            String str = String.valueOf(i);
        }
        Log.e("TAG", "String.valueOf()：" + (System.currentTimeMillis() - startTime) + "ms");

        startTime = System.currentTimeMillis();
        for (int j = 0; j < loopTime; j++) {
            String str = i.toString();
        }
        Log.e("TAG", "Integer.toString()：" + (System.currentTimeMillis() - startTime) + "ms");

        startTime = System.currentTimeMillis();
        for (int j = 0; j < loopTime; j++) {
            String str = i + "";
        }
        Log.e("TAG", "i + \"\"：" + (System.currentTimeMillis() - startTime) + "ms");
    }
```

##2.测试结果

![这里写图片描述](http://img.blog.csdn.net/20170907171653474?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlucGVuZ2ZlaTUyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

##3.结论

把一个基本数据类型转换为String,使用**.toString()**是最快的方式、**String.valueOf**和**toString**差不多、**数据+””**的效率最慢，其实这些执行都是毫秒级，现在处理器的性能都比较强悍，其实影响不大，对于用户来说，没有什么感觉，但是我们可以再写代码的时候养成一个良好的习惯，有时候不用抠这么细，毕竟不是底层的代码，应用层，主要先以实现业务功能为主，这样后续有时间可以优化，记住一点：大行不顾细谨，大理不辞小让！