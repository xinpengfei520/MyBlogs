
# CentOS7 安装 MySQL 8.0

个人服务器搭建一个 MySQL 数据库，来读写一些简单的接口数据处理。

废话不多说，直接撸起。

1.切换到用户目录

```
cd ~
```

2.下载 rpm 包

在官网找到对应的 MySQL 的源，我们选择 **mysql80-community-release-el7-1.noarch.rpm** 社区免费正式发布版8.0

```
wget https://repo.mysql.com//mysql80-community-release-el7-1.noarch.rpm
```

3.安装 rpm 包

```
rpm -ivh mysql80-community-release-el7-1.noarch.rpm
```

4.修改 MySQL 的repo源，使MySQL8.0可用

使用 vim 编辑

```
vi /etc/yum.repos.d/mysql-community.repo
```

找到 [mysql80-community] 确保 enabled=1

修改，保存并退出。

5.安装 mysql-server

```
yum install mysql-server
```

开始下载并安装 MySQL，中间会有几次询问你，一路 yes 即可，等待直到完成。

6.修改 MySQL 的访问权限

```
chown -R root:root /var/lib/mysql 
```

7.重启 MySQL 服务

```
service mysqld restart
```

至此 MySQL8.0 的安装就算完成了。

安装完成后，我们要登录使用 MySQL，但是我们需要访问的密码，安装的时候已经为我们生成了一个随机密码，我们必须重置密码才能使用，因为 MySQL8.0 的密码重还比较麻烦，下一篇我来讲解如何重置密码。