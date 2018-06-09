# error please select android sdk

点app运行时报如下错：

```
Error:please select android sdk
```

且app module上打了个红叉，无法点击运行

提示找不到SDK

打开app.iml文件修改如下：
删除旧的，添加新的一行：

![这里写图片描述](https://img-blog.csdn.net/2018060919280432?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

修改完成后rebuild

app 上的红叉消失，显示正常，点击运行，ok

![这里写图片描述](https://img-blog.csdn.net/20180609192822879?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

更多参考文章：

https://blog.csdn.net/CHITTY1993/article/details/78779125

https://blog.csdn.net/BruceHurrican/article/details/51755512

https://blog.csdn.net/aqi00/article/details/78858336/
