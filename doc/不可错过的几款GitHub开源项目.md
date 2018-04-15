#不可错过的几款GitHub开源项目

工作之余或者周末感觉无聊？不知道干什么？想继续提高技术，但是不知道做什么的同学，看过来，不妨利用闲暇时间来撸几个 GitHub 上还不错的开源项目，本文推荐的开源项目比较适合新手、及对MVP设计模式不太熟练的同学练习之用，下面逐一进行简单介绍。

## 1.ZhiHuMVP

一款采用 MVP 模式的仿知乎客户端。

本项目包含以下知识点：

 - MVP架构思想
 - Retrofit RESTful API 框架的配合
 - RxJava 响应式编程
 - Butter Knife：通过注解绑定 View
 - 夜间模式切换

效果图：

![这里写图片描述](https://img-blog.csdn.net/20180415212934402?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

[https://github.com/fangx/ZhiHuMVP](https://github.com/fangx/ZhiHuMVP)

## 2.ColorfulNews

一个 Material Design 风格的新闻阅读 App，采用 MVP 模式，基于 Dagger2+RxJava+Retrofit2进行开发。

主要用到的开源库：

 - Dagger2
 - RxJava
 - Retrofit2
 - Butterknife
 - Greendao
 - PhotoView
 - Glide
 - Picasso
 - ...

效果图：

![这里写图片描述](https://img-blog.csdn.net/20180415213012428?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

[https://github.com/kaku2015/ColorfulNews](https://github.com/kaku2015/ColorfulNews)

## 3.boxing

Bilibili 开源的基于MVP模式的 Android 多媒体选择器。

特性：

 - 支持自定义UI
 - 支持多/单图片选择和预览，单图裁剪功能
 - 支持gif
 - 支持视频选择功能
 - 提供图片压缩
 - 多图生成gif（checkout feature/gif-encode)

效果图：

![这里写图片描述](https://img-blog.csdn.net/20180415213215868?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

[https://github.com/Bilibili/boxing](https://github.com/Bilibili/boxing)

## 4.MinimalistWeather

MinimalistWeather 是 Android 平台上一款开源天气 App ，目前还在开发中。项目基于 MVP 架构，采用各主流开源库实现。开发此项目主要是为展示各种开源库的使用方式以及 Android 项目的设计方案，并作为团队项目开发规范的一部分。

采用的开源库包括：

 - RxJava
 - Retrofit2
 - OKHttp3
 - ORMLite
 - Dagger2
 - ButterKnife
 - RetroLambda
 - Stetho

本项目还展示了：

 - MVP+RxJava在实际项目中的应用，MVP中RxJava生命周期的管理...；
 - 上述罗列的各种开源框架的使用方法；
 - Java8 Lambda表达式和Stream API的用法；
 - 怎样适配Material Design；
 - ToolBar、RecycleView、CardView、CoordinatorLayout等新控件的用法；
 - Gradle的基本配置（包括签名打包、项目依赖等等）；
 - 如何更好的管理Gradle依赖库的版本；
 - 代码混淆配置；
 - 如何快速开发一款结构清晰、可扩展性强的Android Application。

[https://github.com/BaronZ88/MinimalistWeather](https://github.com/BaronZ88/MinimalistWeather)

## 5.TranslateApp

一个实现 『划词翻译』 功能的 Android 应用 ，可能是目前 Android 市场上翻译效率最高的一款应用。

技术点:

 - 全程使用 Dagger2 对项目进行类依赖管理
 - 项目使用标准的 MVP 架构，关于 MVP 的一篇博客
 - 实现对粘贴板的监听以及访问
 - 无需权限显示悬浮窗（6.0悬浮窗权限适配）
 - 使用 RxJava 探索函数式编程
 - 使用 Retrofit2 进行网络请求
 - 开机自启动
 - Android 6.0 自定义文本操作栏

效果图：

![这里写图片描述](https://img-blog.csdn.net/20180415213307416?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

[https://github.com/maoruibin/TranslateApp](https://github.com/maoruibin/TranslateApp)

## 6.Ghost

微影，一款纯粹的在线视频App，基于Material Design + MVP + Dagger2 + RxJava + Retrofit + Realm + Glide。

技术点：

 - 使用RxJava配合Retrofit2做网络请求
 - 使用RxUtil对线程操作和网络请求结果处理做了封装
 - 使用RxPresenter对订阅的生命周期做管理
 - 使用AndroidEventBus来方便组件间的通信
 - 使用Material Design控件和动画
 - 使用MVP架构整个项目，对应于model、ui、presenter三个包
 - 使用Realm做阅读记录和收藏记录的增、删、查、改
 - 使用Glide做图片的处理和加载
 - 使用RecyclerView实现下拉刷新、上拉加载、侧滑删除、长按拖曳
 - 支持主题设置
 - 包含搜索、收藏、历史等功能

效果图：

![这里写图片描述](https://img-blog.csdn.net/20180415213325438?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

[https://github.com/GeekGhost/Ghost](https://github.com/GeekGhost/Ghost)

## 7.GeekNews

极客日报，一款纯粹的阅读App，基于Material Design + MVP + RxJava2 + Retrofit + Dagger2 + Realm + Glide。

技术点：

 - 使用RxJava配合Retrofit2做网络请求
 - 使用RxUtil对线程操作和网络请求结果处理做了封装
 - 使用RxPresenter对订阅的生命周期做管理
 - 使用RxBus来方便组件间的通信
 - 使用RxJava其他操作符来做延时、轮询、转化、筛选等操作
 - 使用okhttp3对网络返回内容做缓存，还有日志、超时重连、头部消息的配置
 - 使用Material Design控件和动画
 - 使用MVP架构整个项目，对应于model、ui、presenter三个包
 - 使用Dagger2将M层注入P层，将P层注入V层，无需new，直接调用对象
 - 使用Realm做阅读记录和收藏记录的增、删、查、改
 - 使用Glide做图片的处理和加载
 - 使用Fragmentation简化Fragment的操作和懒加载
 - 使用RecyclerView实现下拉刷新、上拉加载、侧滑删除、长按拖曳
 - 使用x5WebView做阅览页，比原生WebView体验更佳
 - 使用SVG及其动画实现progressbar的效果
 - 使用RxPermissions做6.0+动态权限适配
 - 使用Jsoup解析V2EX站点DOM
 - 使用原生的夜间模式、分享、反馈
 - 包含搜索、收藏、检测更新等功能
 - 所有踩到的坑都在项目里做了注释

效果图：

![这里写图片描述](https://img-blog.csdn.net/20180415213349333?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

[https://github.com/codeestX/GeekNews](https://github.com/codeestX/GeekNews)

## 8.MovieGuide

MovieGuide是一款受欢迎、高级别的电影Android应用，展示了预告片和评论等功能。采用了MVP模式、RxJava、Dagger 2 和 Bob Martin 的 Clean 架构方法，并针对平板电脑进行了优化。

效果图：

![这里写图片描述](https://img-blog.csdn.net/20180415213413595?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

[https://github.com/esoxjem/MovieGuide](https://github.com/esoxjem/MovieGuide)

## 9.SimpleNews

基于Material Design和MVP的新闻客户端。

效果图：

![这里写图片描述](https://img-blog.csdn.net/20180415213429841?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

[https://github.com/liuling07/SimpleNews](https://github.com/liuling07/SimpleNews)

## 10.Toutiao

一款第三方今日头条客户端，遵循材料设计(Material Design), 聚合了新闻/段子/图片/视频/头条号内容, 没有广告, 仅仅只有存粹的阅读, 不断完善中, 采用 MVP + RxJava + Retrofit 架构的项目。

主要功能特性：

 - 首页五大模块: 新闻 / 图片 / 视频 / 头条号 / 问答
 - 实现首页的布局和数据的显示
 - 实现自定义新闻栏目顺序
 - 新闻详情页面支持日夜两种主题
 - 显示新闻评论
 - 头条号订阅 查看头条号历史文章 / 视频
 - 搜索功能 热门搜索推荐 搜索历史记录
 - 自定义主题颜色 无缝切换日夜皮肤
 - 视频播放
 - 内置3款 logo 随意互换
 - 动态切换字体大小

主要技术点：

 - 基本遵循 Material Design 设计风格
 - 抓包获取今日头条API
 - 使用 Google 官方 MVP 架构
 - DrawerLayout + NavigationView + BottomNavigationView 搭配使用
 - RxBus 代替 EventBus 进行组件之间通讯
 - ViewPager 搭配 Fragment 懒加载
 - SwipeRefreshLayout 搭配 RecyclerView 下拉刷新上拉加载
 - 自定义 BottomNavigationBehavior 实现上滑隐藏下滑显示
 - RxJava + Retrofit2 + OkHttp3 做网络请求
 - OkHttp3 对网络返回内容做缓存, 还有日志、超时重连、头部消息的配置
 - V层基类的构建, 包括 BaseActivity 和 BaseFragment , 对外提供了相同的接口
 - 使用原生的夜间模式
 - 解决侧滑返回与 View 冲突问题
 - 内置 3 款 Logo, 随意切换
 - 使用 CoordinatorLayout 、 AppBarLayout 、 CollapsingToolbarLayout 、    BottomSheetDialog 等新控件
 - 使用 7.0 新工具 DiffUtil , 不再无脑 notifyDataSetChanged
 - 使用 ItemTouchHelper 实现今日头条的频道排序、频道移动, 参考 ItemTouchHelperDemo
 - 使用 RxBinding 优雅实现搜索请求
 - 使用 AutoDispose 绑定 RxJava 生命周期
 - 使用 Travis Cl 持续集成

效果图：

![这里写图片描述](https://img-blog.csdn.net/20180415213453688?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

[https://github.com/iMeiji/Toutiao](https://github.com/iMeiji/Toutiao)

## 11.PhotoNoter

Material Design风格的开源照片笔记。

主要技术点：

 - 整体项目MVP结构
 - Dagger2
 - 相机部分，API >= 21使用 Camera2 ，API < 21使用 Camera
 - 相机的状态机，聚焦状态机
 - 照片缓存分为两种，一个是大图，一个是小图，小图是相册界面缩略图的时候加载的，大图是查看图片的时候加载的
 - 图片处理
 - 沙盒
 - Activity 退出和进入的动画
 - 一些 UI 的动画，比如 “ 意见反馈”、 “ 语音输入” 这里面的动画
 - 主题设置，沉浸式状态栏（Android 5.0）
 - 切换主题
 - 可以滑动 item 和可以拖放 item 的 ListView（SlideAndDragListView）
 - Android 原生平台的 Markdown，支持 EditText 编辑实时预览 （RxMarkdown）
 - RxJava + RxAndroid（RxCategory/ RxPhotoNote/ RxSandBox/ RxFeedBack/ RxUser）
 - dex分包处理
 - Dex自动分包脚本
 - 使用 AOP 进行 Android 6.0 权限适配
 - NDK && AIDL

效果图：

![这里写图片描述](https://img-blog.csdn.net/20180415213523640?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3hpbnBlbmdmZWk1MjE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

[https://github.com/yydcdut/PhotoNoter](https://github.com/yydcdut/PhotoNoter)

好了，终于介绍完了，由于本文篇幅比较长，建议 「收藏」，欢迎大家评论点赞或者转发，如有更多想法欢迎在下方留言。

重要声明：本文中介绍的所有开源项目所有权仍归原开源作者所有，本文仅对原作者的开源项目进行介绍、整理说明，供参考、学习交流之用。