
<center>点击上面蓝色字体关注<span style="color:#0000ff;">“IT大飞说”</span></center>
<center>置顶公众号（**ID：ITBigFly**）第一时间收到推送</center>
![](http://pcobk8jbf.bkt.clouddn.com/FhaQ3TFtgnEnaDxOllrIjS-VPTAn)
>上一篇文章[深入浅出Retrofit2.x(一)](https://mp.weixin.qq.com/s/jxBEg7dpFfTNLXNFdvJjrw)中，我们讲解了 **Retrofit** 的最基本用法，以及 **GET** 请求示例，非常简单，相信大家都已经会使用了，本篇文章将继续介绍 **Retrofit** 的一些其他的一些常用用法，主要围绕如何使用 **POST** 请求提交表单数据，提交 **json** 字符串作为请求体，还有一些 **Retrofit** 的相关注解介绍等。

## 1.POST 请求我最行

大家在和服务交互的时候，大多情况下都应该使用的是 **GET、POST** 请求吧，下面我们就对 **POST** 各种数据类型及如何定义接口进行说明。

### 1.1 POST 提交表单数据

首先定义一个如下接口，如果是单参数或者是双参数的话可以这么写：

```
   @FormUrlEncoded
   @POST("/")
   Call<ResponseBody> postFormData(
       @Field("name") String name,
       @Field("occupation") String occupation);
```

如果参数较多的话可以使用 **@FieldMap** 注解

```
    // 提交 FORM 表单数据
    @FormUrlEncoded
    @POST("{url}")
    Call<ResponseBody> postFormData(
            @Path("url") String url,
            @FieldMap Map<String, String> params);
```

另外 **@Field** 和 **@FieldMap** 也可以结合在一起使用。

### 1.2 POST 提交 Json 数据

我们大多数情况下，发送 **POST** 请求都是提交 **Json** 数据和服务器进行交互，而 **Json** 数据的序列化方式也有多种，我们可以先把对象序列化成 **Json** 再提交，也可以直接传入一个 **Map** 参数集合等等都是可以的，看似我们没有序列化，其实内部已经帮我自动实现了序列化的过程，对于不同的参数类型，我们索性就定义一个比较通用的 **ApiService** 接口，然后把常用的各种请求方式、文件的上传下载等，以及不同参数类型的方法进行封装，把动态变化的参数暴露给外面，让外部传入即可，接口定义如下，每个方法我都加了注释，看注释就好。

```
/**
 * Created by x-sir on 2018/8/1 :)
 * Function:
 */
public interface ApiService {

    // GET 无参数请求
    @GET("{url}")
    Call<ResponseBody> getRequest(
            @Path("url") String url);

    // GET 带参数请求
    @GET("{url}")
    Call<ResponseBody> getMapParam(
            @Path("url") String url,
            @QueryMap Map<String, String> maps);

    //  POST Map body 请求
    @Headers({"Content-Type: application/json", "Accept: application/json"})
    @POST("{url}")
    Call<ResponseBody> postMapBody(
            @Path(value = "url", encoded = true) String url,
            @Body Map<String, String> maps);

    // POST RequestBody 必须要设置 encoded = true, 否则 url 有编码问题
    // 使用@Path时，path对应的路径不能包含”/”，否则会将其转化为%2F，在遇到想动态的拼接多节url时，还是使用@Url吧
    @Headers({"Content-Type:application/json", "Accept:application/json"})
    @POST("{url}")
    Call<ResponseBody> postRequestBody(
            @Path(value = "url", encoded = true) String url,
            @Body RequestBody body);

    //  POST Url body 请求(适用于动态域名访问，当url为全域名时，会使用url的全域访问，当为非全域时，会拼接到BASE_URL的后面)
    @Headers({"Content-Type: application/json", "Accept: application/json"})
    @POST
    Call<ResponseBody> postUrlBody(
            @Url String url,
            @Body Map<String, String> maps);

    // 提交 FORM 表单数据
    @FormUrlEncoded
    @POST("{url}")
    Call<ResponseBody> postFormData(
            @Path("url") String url,
            @FieldMap Map<String, String> params);

    // 单文件/图片上传
    @Multipart
    @POST("{url}")
    Observable<ResponseBody> upLoadFile(
            @Path("url") String url,
            @Part("image\"; filename=\"image.jpg") RequestBody requestBody);

    // 多文件/图片上传
    @POST("{url}")
    Call<ResponseBody> uploadFiles(
            @Path("url") String url,
            @Path("headers") Map<String, String> headers,
            @Part("filename") String description,
            @PartMap() Map<String, RequestBody> maps);

    // 文件下载
    @Streaming
    @GET
    Observable<ResponseBody> downloadFile(
            @Url String fileUrl);
}
```

然后我们先发送一个 **POST** 请求，是将 **json** 串作为请求体，接口的参数类型是一个 **Map** 集合：

```
Retrofit retrofit = new Retrofit.Builder()
                .baseUrl("https://api.example.com")
				.addConverterFactory(GsonConverterFactory.create())
                .build();

ApiService service = retrofit.create(ApiService.class);
        Call<ResponseBody> call = service.postMapBody(url, map);
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

同样的我们想使用 **Post** 提交 **Form** 表单数据，就只需要调用 **ApiService** 的 **postFormData** 方法即可，其他不用变，这样做的好处是不用定义多个 **Service** 了，只需要定义一个** ApiService** 就可以满足了所有参数类型的请求了，另外 **POST** 还可以支持直接传入一个 **RequestBody** 类型的参数，我们可以直接将对象转换为 **RequestBody** 对象，然后再请求即可，转化的话我们可以定义一个工具类来实现：

```
/**
 * Created by x-sir on 2018/8/3 :)
 * Function:
 */
public class Converter {

    public static RequestBody toBody(Object object) {
        return RequestBody.create(MediaType.parse("application/json; charset=utf-8"),
                new Gson().toJson(object));
    }
}
```

我们来个请求的示例：

```
Retrofit retrofit = new Retrofit.Builder()
                .baseUrl("https://api.example.com")
				.addConverterFactory(GsonConverterFactory.create())
                .build();

ApiService service = retrofit.create(ApiService.class);
        Call<ResponseBody> call = service.postRequestBody(url, Converter.toBody(map));
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

## 2.GET 请求之 @Query 和 @QueryMap 注解

**@Query** 其实相当于我们在请求时 **Url** 中 **‘?’** 后面的 **key-value**，例如：

```
http://api.github.com/?page=1
```

我们就可以这样来定义接口了

```
@GET("/list")
Call<ResponseBody> getPageData(@Query("page") int page);
```

这里的 **page=1** 就是一个 **@Query**，如果我有多个 **@Query**，我们可以使用 **@QueryMap** 使用方法很简单，我就不多说了。

## 3.Retrofit 注解

我们都知道，在 **Retrofit** 中，大量使用了注解，这样做的好处是可以简化请求，通过各种不同的注解来进行灵活的配置，另外也可以达到解耦的目的，所以说 **Retrofit** 的设计很美很优雅， **Retrofit** 的注解大体分为以下几类： 

1. 用于标注请求方式的注解；
2. 用于标记请求头的注解；
3. 用于标记请求参数的注解；
4. 用于标记请求和响应格式的注解；

### 3.1 请求方法注解

|注解|说明|  
| ------ | ------ |  
|@GET|GET 请求|  
|@POST|POST 请求|  	
|@PUT|PUT 请求|  
|@DELETE|DELETE 请求|  
|@PATCH|PATCH 请求，对 PUT 请求的补充，用于更新局部资源|  
|@HEAD|HEAD 请求|  
|@OPTION|OPTION 请求|  
|@HTTP|通用注解，可以替换以上所有的注解，其拥有三个属性：method、path、hasBody|  

### 3.2 请求头注解

|注解|说明|  
| ------ | ------ |  
|@Headers|添加固定请求头，可以同时添加多个并且不会相互覆盖原有的请求头，而是共同存在|  
|@Header|作为方法的参数传入，用于添加不固定值的 Header，该注解会更新覆盖已有的请求头|  

### 3.3 请求参数注解

|注解|说明|  
| ------ | ------ |  
|@Body|多用于 POST 请求发送非表单数据,比如想要以 POST 方式传递 json 格式数据|  
|@Filed|多用于 POST 请求中表单字段，@Filed 和 @FieldMap 需要与 @FormUrlEncoded 注解结合使用|  
|@FiledMap|和 @Filed 作用一致，用于不确定表单参数|  
|@Part|用于表单字段,@Part 和 @PartMap 与 @Multipart 注解结合使用,适合文件上传的情况|   
|@PartMap|用于表单字段,默认接受的类型是 Map，可用于实现多文件上传|  
|@Path|用于 url 中的占位符|  
|@Query|用于 GET 请求的 url 中指定 key=value 参数|  
|@QueryMap|和 @Query 使用类似|  
|@Url|指定请求路径，可以是全路径和相对路径|  

### 3.4 请求和响应格式注解

|注解|说明|  
| ------ | ------ |  
|@FormUrlEncoded|表示请求发送编码表单数据，每个键值对需要使用 @Field 注解|  
|@Multipart|表示请求发送 Multipart 数据，需要配合使用 @Part|  
|@Streaming|表示响应用字节流的形式返回，如果没使用该注解，默认会把数据全部载入到内存中，该注解在在下载大文件的特别有用|  

好了，前两篇的分享已经完了，主要是围绕 Retrofit 最常用最基本的用法来展开讲解的，相信大部分读者看完都已经会用了，大家可能已经看出来这样的代码还是不太好用，没有一些设计的思想，代码比较冗余，下一篇文章将开始讲解如何进行封装一个通用的 Retrofit，这样我们就可以进行方便的进行使用了，提高了代码的复用性和封装性，以及通用性，敬请期待吧！

**猜你喜欢：**  
[给大家推荐一个神器](https://mp.weixin.qq.com/s/ReJJFmQm6l55r9pvo7aFIA)  
[深入浅出Retrofit2.x(一)](https://mp.weixin.qq.com/s/jxBEg7dpFfTNLXNFdvJjrw)  
[不得不会的10点Java基础知识](https://mp.weixin.qq.com/s/H8WqFy2RPYZWRpp63oBPfQ)  
[Android 最全 Intent 传递数据姿势](https://mp.weixin.qq.com/s/hVhhDdr3Bo4qLIdOkHsM_Q) 

![](http://pcobk8jbf.bkt.clouddn.com/Fr_08YggSBgBwGhjjZsX8bH8JYNR)
<center><span style="color:#bdbdbd;">欢迎扫码关注我的公众号</span></center>
