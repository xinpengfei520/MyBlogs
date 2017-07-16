#Elinker dataapp...-1libarmlibserial_port.so has tex

如题，今天开发Android串口时的使用***android-serialport-api***开源库，集成到项目中，然后就崩了，出现了下面的错误：

```java.lang.UnsatisfiedLinkError: dlopen failed: /data/app/com.../lib/arm/libavcodec.so: has text relocations```

从字面的意思大概看出来是：.so库找不到了重定向了

解决方法：

参考：

https://stackoverflow.com/questions/32346402/libavcodec-so-has-text-relocations

https://stackoverflow.com/questions/22577315/warning-linker-app-process-has-text-relocations-this-is-wasting-memory-and-is

https://stackoverflow.com/questions/44870554/ndk-15-breaks-serial-port-library

Thanks.

