#Java中equals与==区别


**==**是运算符而equals是比较方法。

##1、==运算符

（1）基本数据类型比较值:只要两个变量的值相等，即为true
注意：当基本数据类型使用“==”进行比较时，符号两边的数据必须类型兼容（类型相同或可自动类型转换），否则编译出错；

（2）引用类型比较引用：比较两个对象的地址值，两个引用变量指向同一个对象的堆空间时，才为true

```
Person p1=new Person();   
Person p2=new Person();
if(p1==p2)   //false
p1=p2;//如果有这样的赋值语句后  if(p1==p2)   //true
```

注意：当引用类型使用“==”进行比较时，符号两边的数据类型必须兼容（类型相同或有父子类关系），否则编译出错；

```
Person p=new Person();
String str="hello";
if(p==str)//错误，p和String类型不兼容，Person和String类型无父子类关系
Student stu = new Student();//Student extends Person
if(p==stu)//编译通过
```

##2、equals方法

（1）equals()：所有类都继承了Object，也就获得了equals()方法。  
（2）自定义类可以重写equals方法
注意：如果一个类型没有重写过equals方法，那么调用equals等价于==

```
public class Test {
	public static void main(String[] args) {
		Person p1=new Person();
		Person p2=new Person();
		if(p1==p2){
			
		}
		if(p1.equals(p2)){
			//因为这里Person类没重写equals方法，那么p1.equals(p2)等价于p1==p2
		}
	}
}
class Person{
	
}
```

（3）类String及包装类（Wrapper Class）等来说，JDK已经重写过equals方法，重写过的equals方法比较的是两个对象的类型及内容
String str1=new String("hello");
String str2=new String("hello");
if(str1.equals(str2))//true，比较的是字符串的内容。

##3、常量值

（1）字符串常量值

```
		String str1="hello";
		String str2="hello";
		String str3=new String("hello");
		System.out.println(str1==str2);//true
		System.out.println(str1==str3);//false
```

