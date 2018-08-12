<center>点击上面蓝色字体关注“IT大飞说”</center>

![](http://pcobk8jbf.bkt.clouddn.com/FiXJ98D79k-dLqxfCXa4EUuFJAzt)

>在 Android 开发中，从最原生的 **HttpClient** 和后来的 **HttpUrlConnection**，再到**Volley、AsyncHttpClient** 等网络请求框架，最后到 Square 公司开源的 **okhttp**，一时间 **okhttp** 风靡整个开发圈，进而衍生了好多大牛对 **okhttp** 的封装开源库，像鸿洋大神封装的 **okhttputils**，还有 **Okgo** ... 然后就是 **Retrofit** 了，其实它也是对 **okhttp** 的封装，**Retrofit** 使得网络请求更优雅，灵活性更高，支持各种适配器和转换器，可以搭配 **RxJava** 一起使用，一度后来基于 **RxJava + Retrofit + MVP** 这种架构设计非常流行，网上关于这种的文章也更是层出不穷，目前 **retrofit** 在 GitHub 上有将近 3w 的 Star，目前可以说是非常稳定了。

## 1.初识 Retrofit

简单说，**Retrofit** 是一个支持 **RESTful api** 架构的 **http** 网络请求框架的封装库，**Retrofit** 封装了许多自定义的注解，大大方便和简化了使用，另外灵活性也更高，底层采用 **okhttp** 实现真正的网络请求，他们的关系可以说是分工协作吧，总结一成一句话就是：**Retrofit** 让 **okhttp** 网络请求更优雅！

他们的关系可以看下图可能更容易理解：

![](http://pcobk8jbf.bkt.clouddn.com/FhwwxNV6ylKyjVw-T4UKttiY7DZg)

## 2.小牛试刀

### 2.1我们先来个简单的 GET 请求

首先在 Gradle 中添加 Retrofit 的依赖：

```implementation 'com.squareup.retrofit2:retrofit:2.3.0'```

然后我们定义一个接口：

```
public interface GithubService {

    @GET("/users/xinpengfei520")
    Call<ResponseBody> getUserString();
}
```

解释一下上面接口中的注解 **@GET**，这个代表 **HTTP** 的请求类型，同理 **@POST，@PUT，@DELETE,@HEAD,@PATCH** 分别代表 POST、PUT 和 DELETE、HEAD、PATCH 请求；括号中的字符串 **"/users/xinpengfei520"** 代表注解 **@GET** 的值，这里指的是 URL 中的 Path，请求的时候会拼接到BASE_URL 后面，BASE_URL 在构建 Retrofit 的时候设置，**Call<ResponseBody>** 中的 Call 为方法的返回值类型，ResponseBody 为方法的返回值类型的泛型。

接下来，我们构建一个 Retrofit 对象：

```
Retrofit retrofit = new Retrofit.Builder()
                .baseUrl("https://api.github.com")
                .build();
```

非常简单吧，这里使用了构建者模式，方便我们动态的配置各种参数，baseUrl 就是服务器的 HOST，一般是域名或者 ip，通常都带有端口，baseUrl 是一定要设置的，否则会抛异常 ```throw new IllegalStateException("Base URL required.")```。

然后，我们来进行一个异步请求：

```
	GithubService service = retrofit.create(GithubService.class);
        Call<ResponseBody> call = service.getUserString();
        call.enqueue(new Callback<ResponseBody>() {
            @Override
            public void onResponse(Call<ResponseBody> call, Response<ResponseBody> response) {
				String json = response.body().string();
            }

            @Override
            public void onFailure(Call<ResponseBody> call, Throwable t) {

            }
        });
```

上面的代码中先调用 Retrofit 的 create() 方法，方法的参数就是我们定义的接口的 class，create 的内部实现是使用了 Java 中的动态代理模式，考虑到一些基础比较差的同学，关于动态代理，我这里简单的解释一下：代理类在程序运行时创建的代理方式被成为动态代理，代理类并不是在Java代码中定义的，而是在运行时在 Java 代码中动态生成的，当我们使用代理对象执行代理对象的方法时，都会被替换成执行invoke方法，在 invoke 方法中可以拿到代理对象，代理对象的方法及参数，当然这里也涉及到一些反射相关的知识，这里就不详细展开了；结合上面的代码，service 就是代理对象，当 service 调用 getUserString() 方法时，就会触发 invoke 方法，进而通过反射可以拿到代理对象，方法及参数，具体的细节就不详细讲了，感兴趣的同学可以读 Retrofit 的源码进行理解；上面代码中拿到call 后就是网络请求了，一个成功和一个失败的回调方法，这样一次完整的Retrofit的网络请求就算完了。

另外我们还可以使用Retrofit的同步请求接口(注意要放在子线程中请求)：

```
Response<ResponseBody> response = call.execute();
```

### 2.2 优化接口定义

在上面的请求中，很明显我们定义的 GitHubService 接口中的方法的注解的值是写死的，假如我们有另一个请求的接口是 ```https://api.github.com/users/zhangsan``` 也就是说 Path 是 ```/users/zhangsan```，要想请求的话，有两种方式，在之前定义的接口中再增加一个方法，或者另外再重新定义一个接口，这样比较比较麻烦了，通用性和扩展性都不好，和之前定义的接口比较一下，只有 **users/** 后面的内容不同，我们想如果这个可以动态作为传入就好了？当然是可以的，我们可以这样写：

```
public interface GitHubService {
  @GET("/users/{user}")
  Call<ResponseBody> getUserString(@Path("user") String user);
}
```

上面代码中的user作为方法的参数传进来，并且我们使用了@Path注解，然后我们在@GET注解的值中使用 **{user}** 动态获取传进来的 user 的值，这样就达到了动态替换的效果，好了我们又学会了使用
**@Path** 注解了，好棒棒！不过在使用 **@Path** 注解时应该注意一下几点:

- 使用@Path时，Path对应的路径不能包含”/”，否则会将其转化为 %2F，在遇到想动态的拼接多节url时，还是使用@Url；
- @Path只是用来做一层路径，如果路经是多层的，可以用多个@Path，或者用@Url；
- 如果非要使用 @Path 注解来传入多层路径，那就必须指定 @Path的 encoded = true；

### 2.3 进一步优化

通过上面的网络请求测试，感觉和其他网络请求也没什么两样啊，除了使用了 @Path 注解外，好像发现没什么好用的地方？跟你说 Retrofit 能着呢，别人能干的它都能干，别人不能干的它也能干！先别急，我们先慢慢一步一步来了解她，慢慢的你就会发现她的美！就和有的妹子一样，见第一眼觉得不怎么样，但是处的时间长了，你就会发现她越来越漂亮，越看越美...

Retrofit 可以灵活的配置各种转换器，转换器什么鬼？通俗地讲，转换器的意思就是把一种数据类型解析并转换成另一种数据类型，现在不理解没关系，往后面看你就会明白，Retrofit支持的转换器有：

1. Gson: com.squareup.retrofit2:converter-gson
2. Jackson: com.squareup.retrofit2:converter-jackson
3. Moshi: com.squareup.retrofit2:converter-moshi
4. Protobuf: com.squareup.retrofit2:converter-protobuf
5. Wire: com.squareup.retrofit2:converter-wire
6. Simple XML: com.squareup.retrofit2:converter-simplexml
7. Scalars (primitives, boxed, and String): com.squareup.retrofit2:converter-scalars

一般情况下，我们使用 Gson 转换器比较多一些，现在我们就来试试转换器的威力吧。

首先在 Gradle 中添加依赖:

```implementation 'com.squareup.retrofit2:converter-gson:2.3.0'```

接下来我们在创建Retrofit 对象的时候设置它的转换器，代码如下：

```
Retrofit retrofit = new Retrofit.Builder()
                .baseUrl("https://api.github.com")
				.addConverterFactory(GsonConverterFactory.create())
                .build();
```

然后我们创建一个实体类 GithubUserBean，这个类其实就是上面接口返回数据对应的实体类，我们直接用 GsonFormat 插件自动生成即可。

下一步，我们再修改接口中返回值类型的泛型，修改后如下：

```
public interface GithubService {

    @GET("/users/{user}")
    Call<GithubUserBean> getUserString(@Path("user") String user);
}
```

最后，我们再来发起一个异步请求：

```
	GithubService service = retrofit.create(GithubService.class);
        Call<GithubUserBean> call = service.getUserString("xinpengfei520");
        call.enqueue(new Callback<GithubUserBean>() {
            @Override
            public void onResponse(Call<GithubUserBean> call, Response<GithubUserBean> response) {
				GithubUserBean userBean = response.body();
            }

            @Override
            public void onFailure(Call<GithubUserBean> call, Throwable t) {

            }
        });
```

看上面的代码，仔细的你可能已经发现了，当请求接口成功时，我们拿到的直接就是对应的实体类了，不需要再解析了，这是因为 Retrofit 内部会根据这个转换工厂及返回数据所指定的泛型自动帮你直接转换成Bean了，就问你爽不爽？怎一个爽字了得？简直帅呆了！有木有？

注意：接口中返回值类型Call的泛型要么是 ResponseBody，要么是实体类，不支持其他类型！ResponseBody 的子类也不行！

本来打算写一两篇文章来介绍 Retrofit 的，结果发现一两篇文章的根本讲不清楚，因为涉及的知识比较多，另一方面如果是一两篇文章的话，可能篇幅比较长，读者很难坚持读下去，还有我写的时候考虑的东西也比较多，我觉得我的文章应该是面向大众的，尽可能覆盖到大多数的读者，包括一些初学者，由浅入深，由粗到细，这样才能便于大家更好的学习理解，文章中一些东西比较基础，对于觉得会的简单的直接跳过即可，后面的系列文章，我会写一些比较进阶的用法，从源码角度进行分析，便于大家更好的理解。

好了，今天的分享就到这里，我还是你们的大飞哥，一个不止会敲代码的厨师！

推荐阅读：  
[给大家推荐一个神器](https://mp.weixin.qq.com/s/ReJJFmQm6l55r9pvo7aFIA)  
[MySQL8.0设置远程访问权限](https://mp.weixin.qq.com/s/3GlXPLakrVhuq74gOk1wmQ)  
[不得不会的10点Java基础知识](https://mp.weixin.qq.com/s/H8WqFy2RPYZWRpp63oBPfQ)  
[CentOS7 重置 MySQL 8.0 密码](https://mp.weixin.qq.com/s/EwD4Tp9zvaHFfZ17ayTksg)  

<center>大飞哥，带你飞，关注大飞不迷路!</center>
<center>![](http://pcobk8jbf.bkt.clouddn.com/FijAsROsRHuwVUivQ8M9D5o3RSdP)</center>
<center>可能不是最好的公众号，但肯定是最良心的公众号!</center>
