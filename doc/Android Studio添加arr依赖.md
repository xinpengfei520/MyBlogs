#Android Studio添加arr依赖

##1.将arr库放到libs下面

##2.在app的buildgradle的android节点中添加如下代码

```repositories {
        flatDir {
            dirs 'libs' //this way we can find the .aar file in libs folder
        }
    }
```

##3.在dependencies中添加如下依赖

```compile(name: 'com.datescroller.lib-release', ext: 'aar')```

name后面就是arr的库名，不带后缀。