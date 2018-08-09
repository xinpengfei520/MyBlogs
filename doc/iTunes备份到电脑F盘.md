
默认情况下，iTunes 会备份到电脑的 C 盘，当 C 盘快满的时候，iTunes 老是提醒空间不足，导致备份失败，iTunes 也不可以设置将其备份到其他盘，但是我们可以想办法将其备份到其他盘，以前Android手机刚出来的时候，我们的手机配置都比较低，后来由于 APP 越来越大，装的越来越多，后来导致内存不足，安装不了应用，后来使用终端命令 ```ln -s``` 将APP安装目录映射（软连接） ```/data/app/`` 到SD卡的某个目录，这样就解决了内存不足的情况，实现软件装到的是 SD 的目录，主要讲 SD 分出一部分格式化成 ext 日志文件系统，我们都知道 SD 卡都有寿命的，即读写的次数是有限的，这样做会降低手机SD卡的寿命，但是没关系，我们用时间换空间。

继续文章开头的问题，我们想 iTunes 的备份目录是不是也可以这样做呢？答案是可以的。

我们找到默认的 iTunes 的备份目录：

我的是：

```C:\Users\Administrator\AppData\Roaming\Apple Computer\MobileSync\Backup\```

在 Backup 目录下你会看到已经备份的一些文件，没关系，我们在其他盘新建一个文件夹，例如就叫："F:\iTunes_backup" 吧，我们可以先将原来目录备份的文件移动到新建的目录下面，因为这样的话之前已经备份的还在，再次备份时就不用子再从0开始备份了，然后我们使用快键键：win + R 打开window 运行，输入cmd打开 ms shell ,输入如下命令：

```
mklink /D "C:\Users\Administrator\AppData\Roaming\Apple Computer\MobileSync\Backup" "F:\iTunes_backup"
```

然后回车，即建立目录映射链接，相当于是一个快捷方式。

如果执行命令的时候提示 “当文件已存在时，无法创建该文件。”

意思就是已经存在Backup目录了，所以先应该把原目录的Backup目录删掉，因为这个链接就是要创建一个快捷方式，然后再次执行即可。