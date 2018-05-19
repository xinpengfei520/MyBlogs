# JDBC访问MySQL数据库踩坑

作为一个 Android 开发者，闲来无事，想着使用 JSP + Servlet 写一些简单的接口，然后通过前端调用接口，后端的数据库使用的是 MySQL。

## 1.安装MySQL 数据库

安装比较简单，打开 MySQL 官网，我们是个人使用，所以下载最新的（8.0）社区免费版即可。

安装过程中可能需要您设置管理员密码，默认账号为 root,安装完成后就可以启动 MySQL 服务了，Windows下的启动命令如下：

使用管理员权限打开Windows PowerShell

开启MySQL服务： ```net start mysql80```

关闭MySQL服务： ```net stop mysql80```

## 2.连接MySQL服务

我们要创建并操作数据库首先要连接到数据库，连接数据的可视化工具比较多，如 Navicat、 MySQL Workbench、 SQLyog 等，当然，如果你对 MySQL 的命令行比较熟练的话，也可以使用命令来进行相关操作，我个人比较喜欢使用 Navicat。

注意，如果在连接的时候出现连接错误，前提是密码正确，大概报的意思是不支持老的密码方式，也就是说你的可视化工具的版本太低了，使用最新的可视化工具，或者在安装的时候，有两个选项，一个是增强型密码一个是非增强型密码，我们选择非增强型密码即可。

连接上之后我们就可以建数据库建表了，因为我们都是写一些简单的接口，所以不是很复杂我们手动建表，一般实际开发都是使用模型可视化工具来建表，当然表之间的关系也就比较复杂了。

## 3.JDBC访问数据库

因为只是练习使用，所以没使用框架，代码中使用原始的 JDBC 来连接并访问数据库

主要代码如下：

加载驱动，获取连接

```
public DBHelper(String sql) {
		try {
			Class.forName(name);
			conn = DriverManager.getConnection(url, user, password);
			if (conn != null) {
				pst = conn.prepareStatement(sql);
			} else {
				System.out.println("conn is null !");
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
```

在测试的过程中报异常了：

```
conn is null !
```

拿不到数据库连接，我期初判断是不是防火墙问题，后来检查了一下，防火墙没问题，而且我访问的是我本地的数据库，继续检查是不是端口的问题，检查了一下 3306 端口就 mysql 占用了，也没有问题。

很是纳闷，因为使用 Navicat 是可以连接上数据库的，理论上代码应该也是没有问题的，JDBC 连接就几行代码，怎么会有问题？后来网上搜了一大堆，都没有解决问题；后来想了想可能是驱动包太旧了吧，我看了一下，现在使用的是 ```mysql-connector-5.1.8.jar``` ，然后我到网上下载了一个 ```mysql-connector-java-5.1.30-bin.jar``` 驱动包，比之前的新一点，替换掉，运行了一下，还是一样的问题。卡了好几天，然后，我让公司的后台帮忙看看，他说代码也没有什么问题，他说很奇怪，然后他给我一个我们公司的数据库，然后我代码中去连接，是可以拿到数据库连接的，访问没有问题，说明代码也是没有问题的，只能怀疑是数据库的问题了，后来网上搜索了一下，可能是数据库权限的问题，我看了一下数据库用户的权限，都是全选的，也没有问题。

后来还是在网上搜，无意中看到一篇文章 [https://blog.csdn.net/tuna_lxg/article/details/40188659](https://blog.csdn.net/tuna_lxg/article/details/40188659) 大概的意思还是说驱动包太旧了，然后最新的驱动包是可以通过 官网下载 MySQL Connector/J驱动包解压缩后，将 MySQL-connector-Java-x.x-bin.jar拷贝到项目中即可，然后我立即下载了一个安装包，解压后提取出其中的 jar 包，放在项目中，（注意新驱动包的name为“com.mysql.cj.jdbc.Driver” 旧包中没有cj）运行起来，访问接口，报如下异常：

![这里写图片描述](https://img-blog.csdn.net/20180520001234929?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
```
Caused by: com.mysql.cj.exceptions.InvalidConnectionAttributeException: The server time zone value 'ÖÐ¹ú±ê×¼Ê±¼ä' is unrecognized or represents more than one time zone. You must configure either the server or JDBC driver (via the serverTimezone configuration property) to use a more specifc time zone value if you want to utilize time zone support.
```

翻译过来就是：

```
引起的:com.mysql.cj.exceptions。InvalidConnectionAttributeException:服务器时区价值”OÐ¹u±e×¼e±¼的识别或代表多个时区。您必须配置服务器或JDBC驱动程序(通过serverTimezone配置属性)，如果您想要利用时区支持，则需要使用更特殊的时区值。
```

网上搜到一篇博客的解决办法：

[https://www.cnblogs.com/shanelau/p/7041293.html](https://www.cnblogs.com/shanelau/p/7041293.html)

我是按第一种方法，在数据库的URL中加入 ```?serverTimezone=UTC```即可。

然后再次运行项目，访问接口...

哇！泪崩~

终于 OK 了！oh yeah~

看了一下 logcat 中打出如下警告：

```
Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.
```

翻译过来就是：

```
不建议在没有服务器身份验证的情况下建立SSL连接。根据MySQL 5.5.45+、5.6.26+和5.7.6+的要求，如果不设置显式选项，则必须建立默认的SSL连接。您需要通过设置useSSL=false来显式禁用SSL，或者设置useSSL=true并为服务器证书验证提供信任存储。
```

因为本人有轻微强迫症，所以解决办法也比较简单在访问数据库的 URL 中加入 ```&useSSL=false```,然后重新运行项目，OK 警告没了，太爽了！

至此这个问题就算完美解决了，哈哈~

2018-5-20 虐狗节 -。-

![这里写图片描述](https://img-blog.csdn.net/20180520001303960?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)