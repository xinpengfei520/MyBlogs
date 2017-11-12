#Android adb命令

##1.查看当前adb版本

```
$adb version
Android Debug Bridge version 1.0.36
Revision 0e9850346394-android
```

##2.查看当前已连接的设备

```
$adb devices
f414ec93        device
20080411        device
```

##3.安装apk文件

直接安装：
```
$adb install + apk文件的路径
```

一般在安装的时候先敲出**adb install**然后直接将apk文件拖到MS Dos窗口中即可,然后点击回车即可自动安装。

提示：有时候有的安装时会失败，因为有可能包含中文路径，建议最好不要包含中文路径！

保留数据和缓存文件，重新安装apk：
```adb install -r hello.apk```

安装apk到sd卡：
```adb install -s hello.apk```

##4.拉取文件到电脑

```
$adb pull +手机文件路径/目录 + 空格 + 电脑目录 
adb pull /data/data/com.xpf.demo/files/20171031.txt c:/Users/xin/
[100%] /data/data/com.xpf.demo/files/20171031.txt
```

##5.从电脑复制文件到手机

```
$adb push + 电脑文件路径 + 空格 + 手机文件路径/目录
$adb push hello.txt /sdcard/mydir/
```

##6.卸载已安装的app

直接卸载：```adb uninstall <package>```

卸载App但保留数据和缓存文件：
```adb uninstall -k com.xpf.demo```

##7.adb Server

我们在调试的时候，偶尔也会遇到手机adb连接不上的情况，我们先kill掉server在start server就好了
启动adb server ：
```adb start-server```
停止adb server ：
```adb kill-server```

##8.查看手机屏幕分辨率

```
	adb shell wm size
	Physical size: 1080x1920
```

##9.查看adb命令帮助信息
      
```adb help```

##10.在命令行中查看LOG日志

```adb logcat -s 标签名```

温馨提示：加入标签名可以进行log打印过滤，不然打印出来的日志太多不方便查看

##11.发布端口

可以设置任意的端口号作为主机向模拟器或设备的请求端口。

```adb forward tcp:5555 tcp:8000```

##12.查看bug报告

```adb bugreport```

##13.屏幕录像

例如，开始录制屏幕并储存到/sdcard根目录，同时名字为video.mp4

```adb shell screenrecord /sdcard/video.mp4```

##14.关闭设备请求，开启设备

```
adb shell stop
adb shell start
```

##15.设备关机以及重启

```
adb reboot
adb shutdown
```

##16.获取已连接的手机序列号

```adb get-serialno```

##17.获取设备的连接状态

```adb -s f414ec93 get-state```

其中**f414ec93**为手机序列号（获取序列号参考16）

##18.查看目录下的所有文件及文件夹

```adb shell ls```

##19.查看文件夹内容

```adb shell cd <folder> ```

##20.新建文件夹（目录）

```adb shell mkdir path/floldername```

##21.发送文件内容

```adb  shell input text <context>```

##22.通过adb命令发送键盘按键事件

```adb shell input keyevent <keycode>```

##23.获取设备参数信息

```adb shell getprop <key>```

##24.设置设备的参数信息

```adb shell setprop <key> <value>```

##25.使用adb命令进行截屏操作

```adb shell screencap -p <path/file>```

##26.查看内存情况，如果有number表示查看多少条数据

```adb shell top [-m <number>]```

##27.查看进程列表数据

```adb shell ps```

##28.杀死对应pid的进程

```adb shell kill <pid>```

##29.查看指定pid进行的运行状态

```adb shell ps -x <pid>```

##30.查看后台服务信息

```adb shell service list```

##31.查看当前内存占用情况

```adb shell cat /proc/meminfo```

##32.查看io内存分区情况

```adb shell cat /proc/iomen```

##33.将system分区重新挂载为可读写分区

```adb remount```

##34.列出除了系统应用的第三方应用包名

```adb shell pm list packages -3```

##35.清除应用数据与缓存

```adb shell pm clear <packagename>```

好了，就写到这里吧，其实ADB命令有很多，我只整理了一些大家比较常用的一些命令，当然我们有时候要记忆的东西太多了，记不住有些命令，你需要做的就是点击收藏按钮，当用的时候就可以方便自己查找了，如果你觉得文本不错，请您点个赞，码字不易，且行且珍惜！

再小的点赞也是对博主的莫大鼓励！

