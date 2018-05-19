
# CentOS7.x 搭建 GitLab 教程

今天闲来无事，想起之前买了一个阿里云 ECS，一直闲置着没用，一时兴起就想搭个自己的 GitLab 玩玩，GitLab 官网也提供了安装教程，很简单，照着步骤一步步基本没什么问题，可能安装的过程中有一些坑需要自己去解决。

## 1.安装 GitLab

打开 GitLab 官网 [https://www.gitlab.com.cn/installation/#centos-7](https://www.gitlab.com.cn/installation/#centos-7)

根据自己的系统选择，我们选择 CentOS7

![图1](https://img-blog.csdn.net/20180519142033865?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

首先，安装并配置必要的依赖关系，确保打开防火墙的 SSH 和 HTTP 的访问，如果没有，需要您输入下面命令打开

```
sudo yum install -y curl policycoreutils-python openssh-server
sudo systemctl enable sshd
sudo systemctl start sshd
sudo firewall-cmd --permanent --add-service=http
sudo systemctl reload firewalld
```

接下来安装 Postfix 发送邮件通知服务，或者如果您想使用其他方式（SMTP）来发送电子邮件，请跳过此步骤，并在安装了 GitLab 之后配置一个外部SMTP服务器。

```
sudo yum install postfix
sudo systemctl enable postfix
sudo systemctl start postfix
```

添加 GitLab 包仓库并安装包

```
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash
```

接下来，安装 GitLab 包，将 “http://gitlab.example.com” 更改为您想要访问 GitLab 的URL，安装将自动配置并在使用该 URL 来访问 GitLab，HTTPS 需要安装完成后来配置。

温馨提示：此处如果设置为域名访问，如果域名没有备案就会出现无法访问，如果不想备案网站域名，那么此处直接配置为服务器的公网 IP 即可。

因为我配置的是域名，因为我的域名没有备案，所以我访问的时候提示我备案域名，如下图：

![图2](https://img-blog.csdn.net/20180519142400510?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

配置完成后就开始下载并安装，过程大概会持续一个小时左右，视服务器具体带宽而定。

如果 GitLab 已经安装完成，并且你已经配置了域名想要修改配置变成 ip，按如下步骤执行命令：

```
vim  /etc/gitlab/gitlab.rb
```

找到 external_url 设置为你的 IP + 端口即可，注意这里设置的端口不能被占用，默认是 8080 端口，如果 8080 已经使用，请自定义其它端口，并在防火墙设置开放相对应的端口。

然后重置并启动 GitLab 执行：

```
gitlab-ctl reconfigure
gitlab-ctl restart
```

然后执行：

```
gitlab-ctl status
```

查看 GitLab 运行状态是否正常

访问 GitLab 页面：

输入你配置的ip或者域名来访问 GitLab，如果没有什么异常，会出现如下页面

![图3](https://img-blog.csdn.net/20180519142153848?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

第一次访问会提示修改密码，你需要按提示修改即可，默认账户名是 ```root```

至此，GitLab 的安装就算完啦。

## 2.配置 postfix 邮件服务

发邮件系统我们用 Postfix，SMTP 系统默认是开启的，我们先设置关闭 SMTP，开启 Postfix

关闭 SMTP：

```
vim /etc/gitlab/gitlab.rb
```

找到 

```
#gitlab_rails['smtp_enable'] = true
``` 

改为 

```
gitlab_rails['smtp_enable'] = false
```

修改后执行

```
gitlab-ctl reconfigure
```

另一种是关闭 Postfix，设置开启 SMTP，相关教程请参考官网

[https://doc.gitlab.cc/omnibus/settings/smtp.html](https://doc.gitlab.cc/omnibus/settings/smtp.html)

此处以配置 163 邮箱为例：

```
gitlab_rails[‘smtp_enable’] = true 
gitlab_rails[‘smtp_address’] = “smtp.163.com” 
gitlab_rails[‘smtp_port’] = 25 
gitlab_rails[‘smtp_user_name’] = “xxx@163.com” 
gitlab_rails[‘smtp_password’] = “xxx” 
gitlab_rails[‘smtp_domain’] = “163.com” 
gitlab_rails[‘smtp_authentication’] = :login 
gitlab_rails[‘smtp_enable_starttls_auto’] = true
gitlab_rails[‘gitlab_email_from’] = “xxx@163.com” 
user[“git_user_email”] = “xxx@163.com”
```

登录到 GitLab 并添加一个 GitLab 账户，并添加设置用户的邮箱地址，测试是否可以收到邮件通知，如果创建完成，设置的邮箱收到邮件说明邮件服务没有问题，注意如果没有收到邮件，有时候邮箱会拦截邮件，你在垃圾邮件中看看是否邮件被拦截，如果被拦截，你可以将此邮件添加到白名单中。

## 3.其他异常情况处理

另外，如果 访问 GitLab 时的速度非常慢，或者报 502 错误，如下图

![图4](https://img-blog.csdn.net/20180519142248145?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

可能是你的服务器配置太低，GitLab 官方建议装 GitLab 的服务器的配置最低要求为：CPU 2核心 + 4GB可用内存(RAM + Swap)。 如果服务器配置过低，可能导致您访问 GitLab的速度过慢，甚至会出现无响应等状况。


提示 

```
FirewallD is not running
``` 

说明防火墙未开启！

执行 

```
systemctl status firewalld
```

如果出现 

```
Active: inactive (dead)
``` 

表示防火墙未开启。

开启防火墙：

```
systemctl start firewalld
```

再次查看防火墙状态，如果出现 

```
Active: active (running)
``` 

表示防火墙开启成功。

至此，GitLab的安装、配置就完了。

![长按关注](https://img-blog.csdn.net/20180519142917953?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)