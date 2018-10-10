# Hex0 + GitHub

## 1.环境安装

### 1.1 Git 

Windows:
[https://gitforwindows.org/](https://gitforwindows.org/)

如果是 Mac 电脑的话，请先安装 Xcode，因为 Xcode 自带 Git.

### 1.2 Node.js

下载对应系统的 node 安装包，一路 next 无脑安装即可；

[https://nodejs.org/en/](https://nodejs.org/en/)

### 1.3 hexo

因为 npm 命令是 node 中的，所有需要先安装node 然后再安装 hexo，然后利用 npm 命令即可安装 hexo（Windows 在任意位置点击鼠标右键，选择 Git bash，Mac 直接在终端中如下执行命令即可）

```
npm install -g hexo
```

提示本篇文章可能存在时效性，最新版本 hexo 的安装请查看官方文档：

[https://hexo.io/docs/](https://hexo.io/docs/)

报错:

npm ERR! registry error parsing json 错误，可能需要设置 npm 代理，执行命令

```
npm config set registry http://registry.cnpmjs.org
```

hexo:command not found
删除刚刚安装的 npm 目录，重新执行命令：

```
npm install -g hexo
```

来安装 hexo。

## 2.开始搭建

首先创建 hexo 文件夹，如 HexoBlog，注意创建的文件夹一定是空的！！！进入 HexoBlog 目录并执行以下指令(Windows 在 HexoBlog 文件夹内点击鼠标右键，选择 Git bash，Mac 直接 cd 进入)，Hexo 即会自动在文件夹生成网站所需要的所有文件。

```
hexo init
```

安装依赖包:

```
npm install
```

报错：

evernotecid://785B2D88-EE4A-435D-8F13-6593AEC86C1F/appyinxiangcom/15001405/ENResource/p25846![](http://pcobk8jbf.bkt.clouddn.com/FhjB0eR8pfC5j4rXk0pept0hCEwJ)

根据错误提示，是系统没有 **package.json** 这个文件导致。这个文件的作用就是管理你本地安装的**npm** 包，一个 **package.json** 文件可以做如下事情：

1.展示项目所依赖的npm包；
2.允许你指定一个包的版本[范围]；
3.让你建立起稳定，意味着你可以更好的与其他开发者共享；

此刻我们需要执行命令：

```
npm init
```

创建package.json文件，系统会提示相关配置，也可以使用命令：

```
npm init -y
```

直接创建 **package.json** 文件，这样创建好处是必填项已经帮你填好，执行完命令后可以看到用户路径下多了一个 **package.json** 文件。

温馨提示：如果是 Mac 的话，执行每一步命令记得都需要加 sudo !

现在我们已经搭建起本地的 hexo 博客了，执行以下命令，然后到浏览器输入```localhost:4000```就能看到我们搭建的博客了。

```
hexo generate
hexo server
```

好了，至此，我们本地博客已经搭建起来了，这里只能本地访问，别人看不到的，我们搭建的博客如果只是在本地那是没有任何意义的，接下来，我们需要把本地的博客部署到 GitHub 上去。

报错：

执行 ```hexo server``` 提示找不到该指令!

解决办法：

在 **Hexo 3.0** 后 **server** 被单独出来了，需要安装 **server**，安装的命令如下：

```
npm install hexo -server --save
```

安装 **server** 后再试，问题解决~

## 3.GitHub 创建博客

如果已经有 GitHub 账号就创建一个仓库，仓库的名字需要和你的账号对应，格式为: **yourname.github.io** 其中 yourname 就是你的 GitHub 的用户名。

如果没有 GitHub 账号，您需要先注册一个账号，注册完之后需要把你本地机器的 公钥添加到 GitHub 中去。

### 3.1 生成 SSH 密钥

打开 Git Bash 执行如下命令行：

```
ssh-keygen -t rsa -C "youremailaddress"
```

执行完会提示你输入一些信息，这里我们直接一路回车即可，密码设置为空。

### 3.2 查看 SSH 密钥

SSH 公钥默认储存在账户的主目录下的 ~/.ssh 目录，进入 .ssh 目录下，命令行：

```
ls
```

查看如果显示 id_rsa 和 id_rsa.pub，说明已经成功生成了 SSH 公钥。

### 3.3 添加 SSH 密钥

打开 id_rsa.pub（Windows 可用任意一个文本编辑器打开，Mac 下用 cat 或者 vim 编辑器查看）复制里面的全部内容到:

[https://github.com/settings/ssh](https://github.com/settings/ssh) 

找到 Add SSH key，粘贴进去并保存～

添加 ssh 的作用是你的 GitHub 就可以和你本地的机器进行 push 和 pull 操作了，不需要输入任何的密码，GitHub 就已经知道你是操作的了。

## 4.Hexo 的使用

### 4.1 Hexo 的目录结构

![](http://pcobk8jbf.bkt.clouddn.com/Fpjfqr-LghTX4LF8Ivk4U06WdRNo)

### 4.2 全局配置文件_config.yml

```
# Hexo Configuration
# Docs: http://hexo.io/docs/configuration.html
# Source: https://github.com/hexojs/hexo/
# Site #站点信息
title:  #标题
subtitle:  #副标题
description:  #站点描述，给搜索引擎看的
author:  #作者
email:  #电子邮箱
language: zh-CN #语言
# URL #链接格式
url:  #网址
root: / #根目录
permalink: :year/:month/:day/:title/ #文章的链接格式
tag_dir: tags #标签目录
archive_dir: archives #存档目录
category_dir: categories #分类目录
code_dir: downloads/code
permalink_defaults:
# Directory #目录
source_dir: source #源文件目录
public_dir: public #生成的网页文件目录
# Writing #写作
new_post_name: :title.md #新文章标题
default_layout: post #默认的模板，包括 post、page、photo、draft（文章、页面、照片、草稿）
titlecase: false #标题转换成大写
external_link: true #在新选项卡中打开连接
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
highlight: #语法高亮
  enable: true #是否启用
  line_number: true #显示行号
  tab_replace:
# Category & Tag #分类和标签
default_category: uncategorized #默认分类
category_map:
tag_map:
# Archives
2: 开启分页
1: 禁用分页
0: 全部禁用
archive: 2
category: 2
tag: 2
# Server #本地服务器
port: 4000 #端口号
server_ip: localhost #IP 地址
logger: false
logger_format: dev
# Date / Time format #日期时间格式
date_format: YYYY-MM-DD #参考http://momentjs.com/docs/#/displaying/format/
time_format: H:mm:ss
# Pagination #分页
per_page: 10 #每页文章数，设置成 0 禁用分页
pagination_dir: page
# Disqus #Disqus评论，替换为多说
disqus_shortname:
# Extensions #拓展插件
theme: landscape-plus #主题
exclude_generator:
plugins: #插件，例如生成 RSS 和站点地图的
- hexo-generator-feed
- hexo-generator-sitemap
# Deployment #部署，将 lmintlcx 改成用户名
deploy:
  type: git
  repo: git@github.com:yourgithub/yourgithub.github.io.git
  branch: master
```

编辑 **config.yml** 文件将 repo: git@github.com:yourgithub/yourgithub.github.io.git 替换为你刚建的 GitHub 仓库的地址。

注意：
每一行配置文件的冒号 “:” 后面有一个空格！
repo 的这种形式的是配置了SSH Key之后的，如果没有配置则使用 https 形式的地址。

然后执行如下命令即可完成生成静态网页 & 部署：

```
hexo generate
hexo deploy
```

提示：每次在执行上面两条命令之前先 hexo clean 一下，这样可以防止缓存的影响。

如上命令可以简写，简写和全写命令对应如下：

```
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```

问题：

![](http://pcobk8jbf.bkt.clouddn.com/FiSI_f8EBlBMHZXLfeqVa0kTQvt8)

安装 deployer:

```
npm install hexo-deployer-git --save
```

再重新 hexo d，如果显示以下提示说明部署成功：

```
[info] Deploy done: git
```

然后打开浏览器输入：yourgithub.github.io

如果你看到了和刚才本地访问一样的网页，说明我们的博客已经成功的部署到了 GitHub 了。

## 5.绑定域名

博客搭建完，默认的域名只能是：yourgithub.github.io 而且是不能修改的，如果要想逼格更高点，想绑定自己的域名也是可以的。

### 5.1 购买域名

首先你需要购买一个域名，现在基本都是到万网或者腾讯等也可以购买域名，购买也比较简单，这里放出阿里万网购买地址：

[https://wanwang.aliyun.com/](https://wanwang.aliyun.com/)

### 5.2 配置DNS地址

这里以万网为例，进入万网的管理控制台(阿里云手机 APP 也可以)进行修改，修改 DNS 为 DNSPod 的免费 DNS 地址：

```
f1g1ns1.dnspod.net
f1g1ns2.dnspod.net
```

### 5.3 域名解析

登录万网控制台或阿里云手机 APP 之后，把我们新注册的域名加进去，在域名解析列表中国添加 3 条解析记录：

```
@         A         192.30.252.153
@         A         192.30.252.154
www      CNAME      yourname.github.io
```

其中A的两条记录指向的 ip 地址是 GitHub Pages 的提供的 ip，如何知道你的 GitHub 上项目的 ip？

### 5.4 添加 CNAME 文件

新建一个名为 CNAME 的文件，无后缀，内容为你的域名地址，将该文件放入本地博客的 source 文件夹里面，执行下面命令并更新到 Github。

```
hexo clean
hexo g
hexo d
```

注意：域名的地址不需要添加协议头 **http://** ,直接写域名即可，例如：www.baidu.com

然后在你的浏览器中输入你的域名，是不是和之前一样，如果一样，说明你的域名已经绑定成功了～

搭建博客本身的难度不大，重要的是搭建博客这个过程，主要考验自己的动手能力和问题解决能力，过程中你可能会遇见各种各样的问题，然后你会一个个的解决掉这些问题，这本身就是学习的过程，希望大家也能多多动手，希望大家搭建成功～

最后晒上我的博客地址：

[http://www.x-sir.com](http://www.x-sir.com)

## 6.Hexo 博客主题美化

### 6.1 clone 主题

Hexo官网：

[https://hexo.io/themes](https://hexo.io/themes)

里面有特别多的主题可以选择，我在这里选的是 next这个主题，下载主题：

```
cd themes
git clone https://github.com/Fechin/hexo-theme-diaspora.git diaspora
```

### 6.2 启用主题

修改Hexo配置文件 _config.yml 主题项设置为diaspora

```
...
theme: diaspora
...
```

### 6.3 更新主题

注意：请在更时主题时备份_config.yml配置文件

```
cd themes/diaspora
git pull
```

### 6.4 发布新主题

执行如下命令然后浏览器中输入博客地址查看：

```
hexo clean
hexo g
hexo d
```

注意：更换新的主题，可能会有一些延迟！

## 7.Hexo 博客细节配置

需要修改的内容可以参考网站：

[https://hexo.io/zh-cn/docs/configuration](https://hexo.io/zh-cn/docs/configuration)

里面对各个参数有一些详细的解释，我这里修改一些博客的标题，副标题，描述，语言等，建议大家修改一项，重新部署网站一下，因为一旦出错，这样就比较容易找到原因。

## 8.安装插件

```
安装插件：npm install 插件名 –save
卸载插件：npm uninstall 插件名
更新插件和博客框架：npm update
```

执行以下命令安装 RSS 插件:

```
npm install hexo-generator-feed --save
```

生成站点地图:

```
npm install hexo-generator-sitemap --save
```

生成百度站点地图:

```
npm install hexo-generator-baidu-sitemap --save
```

SEO 优化:

```
npm install hexo-generator-seo-friendly-sitemap --save
```

HTML 压缩:

```
npm install hexo-html-minifier --save
```

CSS 压缩:

```
npm install hexo-clean-css --save
```

JS 压缩:

```
npm install hexo-uglify --save
```

imagages 压缩:

```
npm install hexo-imagemin --save
```

插件开启配置

根目录下的 `_config.yml`，添加以下代码:

```
# RSS
feed:
type: atom
path: atom.xml
limit: 20
# sitemap
# 提交给谷歌搜素引擎，SEO优化开启配置是一样的
sitemap:
    path: sitemap.xml
# 提交百度搜索引擎   
baidusitemap:
    path: baidusitemap.xml 
# HTML压缩
html_minifier:
  exclude:     
# css压缩
clean_css:
  exclude: 
	- '*.min.css'
# js压缩   
uglify:
  mangle: true
  output:
  compress:
  exclude: 
	- '*.min.js'
# image压缩    
imagemin:
  enable     : true
  interlaced : false
  multipass  : false
  optimizationLevel: 2
  pngquant   : false
  progressive: false
```
  
注意：目前设置 HTML 压缩有报错！
图片压缩会导致图片破损！
package.json 文件中可以看安装了哪些插件。

## 9.安装 gitalk 评论插件

参考下面的博客即可，写的比较详细：

[https://segmentfault.com/a/1190000014085547](https://segmentfault.com/a/1190000014085547)

如果安装过程中，出现问题请先戳gitalk issue 区看看有没有解决方案：

[https://github.com/gitalk/gitalk/issues](https://github.com/gitalk/gitalk/issues)

### 参考链接

* [https://hexo.io/docs/](https://hexo.io/docs/)
* [http://wuxiaolong.me/2015/07/31/build-blog-by-hexo/](http://wuxiaolong.me/2015/07/31/build-blog-by-hexo/)
* [https://www.jianshu.com/p/e5f95eb990ad](https://www.jianshu.com/p/e5f95eb990ad)
* [https://github.com/Fechin/hexo-theme-diaspora](https://github.com/Fechin/hexo-theme-diaspora)
* [https://www.jianshu.com/p/77db3862595c](https://www.jianshu.com/p/77db3862595c)
* [https://www.jianshu.com/p/86557c34b671](https://www.jianshu.com/p/86557c34b671)
* [https://github.com/gitalk/gitalk](https://github.com/gitalk/gitalk)
* [https://www.jianshu.com/p/9be29ed2f4b7](https://www.jianshu.com/p/9be29ed2f4b7)
* [https://imsun.net/posts/gitment-introduction/#more](https://imsun.net/posts/gitment-introduction/#more)
* [https://calpa.me/2018/03/10/gitalk-error-validation-failed-442-solution](https://calpa.me/2018/03/10/gitalk-error-validation-failed-442-solution)