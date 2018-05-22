
上一篇文章讲解了重置 MySQL 的密码，有同学反馈无法程连接到数据库，这是因为 MySQL 安装完成后只支持 localhost 访问，我们必须设置一下才可以远程访问，另外还有一些 MySQL 8.0 连接时的一些问题，本文也会一并进行讲解如何解决。

1.登录MySQL 

```
mysql -u root -p
```

输入您的密码

2.选择 mysql 数据库

```
use mysql;
```

因为 mysql 数据库中存储了用户信息的 user 表。

3.在 mysql 数据库的 user 表中查看当前 root 用户的相关信息

```
select host, user, authentication_string, plugin from user; 
```

执行完上面的命令后会显示一个表格

查看表格中 root 用户的 host，默认应该显示的 localhost，只支持本地访问，不允许远程访问。

4.授权 root 用户的所有权限并设置远程访问

```
GRANT ALL ON *.* TO 'root'@'%';
```

GRANT ALL ON 表示所有权限，% 表示通配所有 host，可以访问远程。

5.刷新权限

所有操作后，应执行

```
flush privileges;
```

6.查看 root 用户的 host

再次执行步骤 2，你会发现 root 用户的 host 已经变成 %，说明我们的修改已经成功，可以远程访问了。

7.访问数据库

远程访问数据库的可视化工具比较多如：Navicat、SQLyog、MySQL workbench 等，我这里使用 Navicat

输入访问的 host 和密码，报 2059 错误，这是因为 MySql8.0 版本 和 5.0 的加密规则不一样，而现在的可视化工具只支持旧的加密方式。

此问题有两种方法，一种是更新 Navicat 驱动来解决此问题，另一种是将 MySQL 用户登录的加密规则修改为 mysql_native_password，第一种方法我试过了没有起作用，我这里采用第二种方法。

8.修改加密规则  

```
ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER; 
```

password 为你当前密码。

9.更新 root 用户密码 

```
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'password'; 
```

password 为你新设置的密码。

10.刷新权限

```
FLUSH PRIVILEGES; 
```

OK，设置完成，再次使用 Navicat 连接数据库，oh yeah~

完美解决！



