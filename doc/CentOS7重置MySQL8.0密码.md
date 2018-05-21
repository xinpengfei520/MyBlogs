
# CentOS7 重置 MySQL 8.0 密码

上一篇文章我们已经讲完了 MySQL 的安装，本篇接下来将一下如何重置密码。

1.查看自动生成的密码

安装完成后 MySQL会给我们自动生成一个随机密码

查看命令如下：

```
grep 'temporary password' /var/log/mysqld.log
```

因为自动的生成的密码无法直接使用，也不便于我们记忆，所以我们要修改密码。

2.配置文件 MySQL 免密码登录

编辑 MySQL 的配置文件 

```
vim /etc/my.cnf
```

在 pid 开头的下面一行加入下面这句 

```
skip-grant-tables
```

保存并退出。

3.重启 MySQL 服务

```
service mysqld restart 
```

4.免密码登录到 MySQL 上

```
mysql -u root -p
```

提示输入密码时直接敲回车。

5.选择 mysql 数据库

```
use mysql;
```

因为 mysql 数据库中存储了一张 MySQL 用户的 user 表

6.在 mysql 数据库的 user 表中查看当前 root 用户的相关信息

```
select host, user, authentication_string, plugin from user; 
```

执行完上面的命令后会显示一个表格

表格中有以下信息：

host: 允许用户登录的 ip ‘位置’ % 表示可以远程；

user: 当前数据库的用户名；

authentication_string: 用户密码（在mysql 5.7.9以后废弃了password字段和password()函数）；

plugin： 密码加密方式；

7.将默认的 root 密码置空

```
use mysql;  
update user set authentication_string='' where user='root'; 
```

8.退出 mysql命令行

```
quit
```

9.删除 /etc/my.cnf 文件最后的 skip-grant-tables

```
vim /etc/my.cnf
```

删除 skip-grant-tables 并保存退出。

10.重启 MySQL 服务

```
service mysqld restart 
```

11.重新登录到 MySQL 上

```
mysql -u root -p
```

提示输入密码时直接敲回车，因为我们刚才已经将密码置为空了。

12.使用 ALTER 修改 root 用户密码

```
ALTER user 'root'@'localhost' IDENTIFIED BY 'Xpf123@';
```

其中 Xpf123@ 为你设置的新密码，注意这个密码如果设置的比较简单，例如 123456 等等，会设置不成功，它会提示你设置的密码太简单，最好设置成大写字母、数字、符号的组合。

执行完之后会提示你 OK 的话，就代表修改成功了，至此重置密码也就算是完了，你可以使用新设置的密码去登录试试。

可能很多同学在修改的密码中遇到了很多问题，例如装的是 MySQL 8.0，然后去重置密码，还是用的旧的命令去修改密码，导致报错，因为 MySQL 5.7.6 以后废弃了 user 表中的 password 字段和 password() 方法，所以使用旧的方法去重置密码对 mysql 8.0 是不行的！

好了，今天的讲解就到这里吧。

如果大家有什么问题，请在下方留言！

