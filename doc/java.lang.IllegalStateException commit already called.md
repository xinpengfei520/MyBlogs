#java.lang.IllegalStateException commit already called

今天使用Fragment时报如下图错误：

![这里写图片描述](http://img.blog.csdn.net/20171209175100029?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveGlucGVuZ2ZlaTUyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 分析原因

之所以报该错误，是因为 **Fragment** 事务是全局的变量，只能commit一次。

## 解决方法：

将获取Fragment事务定义为局部变量，每次提交时都去获取新的事务去commit即可。