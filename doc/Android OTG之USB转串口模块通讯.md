
#<span  style="color:#1e819e;">Android OTG之USB转串口模块通讯</span>

>微信公众号：CodingAndroid  
CSDN：[http://blog.csdn.net/xinpengfei521](http://blog.csdn.net/xinpengfei521)

##1.背景简介

我们公司开发了一款室内机平板APP应用，要求平板能去控制智能门锁、等其他智能设备，智能门锁不是我们公司开发的，与我们公司属于合作关系。

##2.分析及实现思路
	
1. 智能门锁的控制是通过使用 **433射频**（不了解的请百度）来进行通讯的；
2. 平板是没法与智能门锁直接进行通讯，但是厂家提供了一个433通讯模块（支持串口）；
3. 而平板（支持OTG）是支持USB转串口模块的，所以整个流程是可以走通的，如下图

![这里写图片描述](https://img-blog.csdn.net/20180401005034848?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

##3.主要代码实现

###3.1初始化USB转串口模块

由于我们选用的是CH340模块，我们先导入ch340的 **jar** 包，然后在代码中检查手机/平板是否支持USB HOST模式，如果支持我们就初始化通讯时的相关参数：波特率、数据位、停止位等，具体的参数看你们之间通讯的协议。初始化完成之后，我们就可以打开USB进行通讯了，同时初始化完成之后我们需要开启一个读取数据的线程，这样，一旦收到数据或者相应的响应包（一般也叫ACK）我们就可以进行相应的处理了。

下面贴上初始化、及开启读取数据线程的代码实现：

```
/**
     * initialize ch340 parameters.
     *
     * @param context Application context.
     */
    public static void initCH340(Context context) {
        if (context == null) return;
        Context appContext = context.getApplicationContext();
        mUsbManager = (UsbManager) appContext.getSystemService(Context.USB_SERVICE);
        if (mUsbManager != null) {
            HashMap<String, UsbDevice> deviceHashMap = mUsbManager.getDeviceList();
            for (UsbDevice device : deviceHashMap.values()) {
                if (device.getProductId() == 29987 && device.getVendorId() == 6790) {
                    mUsbDevice = device;
                    if (mUsbManager.hasPermission(device)) {
                        loadDriver(appContext, mUsbManager);
                    } else {
                        if (listener != null) {
                            listener.result(false);
                        }
                    }
                    break;
                }
            }
        }
    }

    /**
     * load ch340 driver.
     *
     * @param appContext
     * @param usbManager
     */
    public static void loadDriver(Context appContext, UsbManager usbManager) {
        driver = new CH34xUARTDriver(usbManager, appContext, ACTION_USB_PERMISSION);
        // 判断系统是否支持USB HOST
        if (!driver.UsbFeatureSupported()) {
            InLog.e(TAG, "Your mobile phone does not support USB HOST, please change other phones to try again!");
        } else {
            openCH340();
        }
    }

    /**
     * config and open ch340.
     */
    private static void openCH340() {
        int ret_val = driver.ResumeUsbList();
        InLog.d(TAG, ret_val + "");
        // ResumeUsbList方法用于枚举CH34X设备以及打开相关设备
        if (ret_val == -1) {
            InLog.d(TAG, ret_val + "Failed to open device!");
            driver.CloseDevice();
        } else if (ret_val == 0) {
            if (!driver.UartInit()) {  //对串口设备进行初始化操作
                InLog.d(TAG, ret_val + "Failed device initialization!");
                InLog.d(TAG, ret_val + "Failed to open device!");
                return;
            }
            InLog.d(TAG, ret_val + "Open device successfully!");
            if (!isOpenDeviceCH340) {
                isOpenDeviceCH340 = true;
                configParameters();//配置ch340的参数、需要先配置参数
            }
        } else {
            InLog.d(TAG, "The phone couldn't find the device！");
        }
    }

    /**
     * config ch340 parameters.
     * 配置串口波特率，函数说明可参照编程手册
     */
    private static void configParameters() {
        if (driver.SetConfig(baudRate, dataBit, stopBit, parity, flowControl)) {
            InLog.d(TAG, "Successful serial port Settings！");
            if (readDataRunnable == null) {
                readDataRunnable = new ReadDataRunnable();
            }
            mThreadPool.execute(readDataRunnable);
        } else {
            InLog.d(TAG, "Serial port Settings failed！");
        }
    }
```

###3.2发送和接收数据

发送和接收数据都是按16进制进行发送和处理的，所以我们写了一个工具类方便发送，如下：

```
/**
 * Created by xpf on 2018/2/6 :)
 * Function:CH340数据处理工具类
 */

public class CH340Util {

    /**
     * write data in ch340.
     *
     * @param byteArray 字节数组
     * @return 返回写入的结果，-1表示写入失败！
     */
    public static int writeData(@NonNull byte[] byteArray) {
        // 将此处收到的数组转化为HexString
        String hexString = bytesToHexString(byteArray, byteArray.length);
        InLog.i("TAG", "WriteHexString===" + hexString);
        return InitCH340.getDriver().WriteData(byteArray, byteArray.length);
    }

    /**
     * byte[]转换为hexString
     *
     * @param buffer 数据
     * @param size   字符数
     * @return 返回转换后的十六进制字符串
     */
    public static String bytesToHexString(byte[] buffer, final int size) {
        StringBuilder stringBuilder = new StringBuilder("");
        if (buffer == null || size <= 0) return null;
        for (int i = 0; i < size; i++) {
            String hex = Integer.toHexString(buffer[i] & 0xff);
            if (hex.length() < 2) stringBuilder.append(0);
            stringBuilder.append(hex);
        }
        return stringBuilder.toString();
    }
}
```

然后基本的发送和接收数据就写完了，发送数据时调用 **writeData()** 方法就可以了，接收数据也类似，具体的业务及通讯的协议、加密规则等都需要你和硬件提供方进行协商对接制定，由于涉及到公司机密，此处我就不进行说明了，只说一下最基本的发送和接收数据。

##4.插入模块实现自动打开APP

Android中USB的插拔都会发送一个广播，我们只需要在AndroidMenifest.xml文件中接收这个广播就可以了，当我们入模块的时候就会启动我们的APP,第一次会弹出一个对话框询问我们是否打开xxx应用，我们点击确认即可，另外不是我们插入任何USB设备都打开我们的应用，所以我们需要过滤掉对我们没有用的设备，在res下建一个xml目录，新建usb_filter.xml文件，配置好我们这个USB设备模块的product-id和vendor-id，每个模块厂家这个值都是不一样的，有两种获取方式，一个是代码中枚举USB设备然后打印出来，另外一种方法是打开Logcat观察，然后插入USB设备，你会发现系统会打印出来这个USB设备等信息。

最后不要忘了添加权限：

```
	<uses-feature
        android:name="android.hardware.usb.host"
        android:required="true" />

    <uses-permission android:name="android.hardware.usb.host" />
```

此处由于篇幅原因就不具体展开讲解更多细节了，核心的代码和实现我都已经贴出来了，要想查看更具体的实现，请下载我的demo:

[https://github.com/xinpengfei520/USB-OTG-CH340-UART-interface](https://github.com/xinpengfei520/USB-OTG-CH340-UART-interface)


若在阅读过程中遇到什么问题，或有好提议，欢迎在公众号“CodingAndroid”中提出
![这里写图片描述](http://img.blog.csdn.net/20170911223615073)

长按后点击扫一扫关注！