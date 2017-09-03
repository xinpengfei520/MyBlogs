
#Android Studio打JAR包

Android Studio新增了依赖**library**的方式，有时候我们需要将一个library打成jar包供其他人使用下面就是具体打成的方法：

##1.在library的buildgradle中添加如下代码

```
task makeJar(type: Copy) {
    delete 'build/libs/myjar.jar'
    from('build/intermediates/bundles/release/')
    into('build/libs/')
    include('classes.jar')
    rename('classes.jar', 'myjar.jar')
}
```

其实，当你编译完整个工程后，所有library库都已经生成了对应的包含class的classes.jar包，复制出这个jar包，修改名字后也可以使用了，默认生成的路径为：build/intermediates/bundles/release下面就可以看到生成的**classes.jar**文件了。

##2.如果你不想使用如上编译时生成的Jar包，你可以再Android Studio自带的Terminal中输入如下命令生成jar包
在终端执行生成JAR包  
```./gradlew build```

或者在buildgradle中再添加如下两行代码即可    

```
makeJar.dependsOn(build)   
//build.finalizedBy makeJar
```

Android Studio新增了@arr的依赖方式，它的好处是可以将资源文件也可以打进去，所以，我更推荐大家使用这种依赖方式。


最后他特别感谢如下博主的文章：

参考：blog:http://blog.csdn.net/ta893115871/article/details/46955791