
<center>点击上面蓝色字体关注<span style="color:#0000ff;">“IT大飞说”</span></center>
<center>置顶公众号（**ID：ITBigFly**）第一时间收到推送</center>

![](http://pcobk8jbf.bkt.clouddn.com/FqPzMTldx0N8CIpaF54OfAOggiv8)

>作为一个 **Android** 开发人员，我们都是用过 **Intent**，用它来在组件之间传递数据，所以说 **Intent** 是组件之间通信的使者，一般情况下，我们传递的都是一些比较简单的数据，并且都是基本的数据类型，写法也比较简单，今天我在这里说的是如何使用 **Intent** 传递对象及集合，我们知道**Intent** 是不能直接传递没有序列化的对象的，说到序列化，我们都知道，序列化有两种方式，即实现 **Sereriable** 或者 **Paracelable** 接口。默认情况下，像 **List、Bitmap** 等默认帮我们已经实现了序列化，我们就可以直接进行传递，还有一些像 **Map** 集合，自定义的 **class**，默认是没有实现序列化的接口的，我们必须要先实现序列化才可以进行传递。

## <span style="color:#00ff00;">1.传递序列化对象</span>

### <span style="color:#0000ff;">1.1 方式一</span>

这种方式比较简单，我们可以先将对象使用 **Gson** 先序列化成 **Json** 字符串，然后作为字符串来使用 **Intent**，这种方式的好处是不需要实现 **Sereriable** 或者 **Paracelable**，坏处就是需要额外的使用 **Gson** 来序列化和解析。

代码示例：

**ActivityA** 中设置数据：

```
	User user = new User();
	user.setName("Jack");
	user.setAge(18);
	Intent intent=new Intent(ActivityA.this,ActivityB.class);
	intent.putExtra("user",new Gson().toJson(user));
	startActivity(intent);
```

**ActivityB** 中获取数据：

```
	String json = getIntent().getStringExtra("user");
	User user = new Gson().fromJson(json,User.class);
```

### <span style="color:#0000ff;">1.2 方式二</span>

这种方式就是将数据封装到 **Bundle** 中然后把 **Bundle** 对象调用 **Intent** 的 **putExtra** 方法然后传递过去，**Bundle** 类默认也是已经实现了 **Parcelable** 接口的，所以可以传递 **Bundle** 对象。

代码示例：

**ActivityA** 中设置数据：

```
 	// 创建一个Bundle对象封装数据
    Bundle data = new Bundle();
    data.putInt("age",18);
    data.putString("name","Jack");
	intent.putExtra("data",data);
```

**ActivityB** 中获取数据：

```
Bundle data = getIntent().getBundleExtra("data");
int id = data.getInt("age");
String name = data.getString("name");
```

### <span style="color:#0000ff;">1.3 方式三</span>

传递实现了 **Serializable** 接口的对象，这种方式也比较简单，传递之前先实现 **Serializable** 接口，也不需要重写方法。

代码示例：

**ActivityA** 中设置数据：

```
	User user = new User();
	user.setName("Jack");
	user.setAge(18);
	Intent intent=new Intent(ActivityA.this,ActivityB.class);
	Bundle bundle = new Bundle();
	bundle.putSerializable("user", user);
	intent.putExtras(bundle);
	startActivity(intent);
```

**ActivityB** 中获取数据：

```
Intent intent = getIntent(); 
User user = (User)intent.getSerializableExtra("user");
```

### <span style="color:#0000ff;">1.4 方式四</span>

传递实现了 **Parcelable** 接口的对象，这种方式比实现 **Serializable** 接口的方式稍微麻烦一点，需要重写方法，不过我们程序员都是比较懒的，给大家推荐一个插件： **android-parcelable-intellij-plugin** ，安装完之后就可以使用快捷键自动生成实现了 **Serializable** 接口的对象了，是不是比较方便。

实现 **Serializable** 对象的 **User** 类示例代码如下：

```
public class User implements Parcelable {

    private String name;
    private int age;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public int describeContents() {
        return 0;
    }

    @Override
    public void writeToParcel(Parcel dest, int flags) {
        dest.writeString(this.name);
        dest.writeInt(this.age);
    }

    public User() {
    }

    protected User(Parcel in) {
        this.name = in.readString();
        this.age = in.readInt();
    }

    public static final Parcelable.Creator<User> CREATOR = new Parcelable.Creator<User>() {
        @Override
        public User createFromParcel(Parcel source) {
            return new User(source);
        }

        @Override
        public User[] newArray(int size) {
            return new User[size];
        }
    };
}
```

传递数据的方法和 **Serializable** 类似，还是写一下把：

代码示例：

**ActivityA** 中设置数据：

```
	User user = new User();
	user.setName("Jack");
	user.setAge(18);
	Intent intent=new Intent(ActivityA.this,ActivityB.class);
	Bundle bundle = new Bundle();
	bundle.putParcelable("user", user);
	intent.putExtras(bundle);
	startActivity(intent);
```

**ActivityB** 中获取数据：

```
Intent intent = getIntent(); 
User user = (User)intent.getParcelableExtra("user");
```

## <span style="color:#00ff00;">2.传递集合类</span>

### <span style="color:#0000ff;">2.1 传递 List 集合数据</span>

如果我们要传递的 **List** 集合，我们可以把 **List** 强转成 **Serializable** 类型，**List** 默认是实现了 **Serializable** 接口的，但是注意 **List** 的泛型类也必须要实现了 **Serializable** 接口，基本类型及包装类就不用了。

代码示例：

**ActivityA** 中设置数据：

```
	User user1 = new User();
	user1.setName("Jack");
	user1.setAge(18);
	User user2 = new User();
	user2.setName("Marry");
	user2.setAge(20);

	List<User> list = new ArrayList<>();
    list.add(user1);
    list.add(user2);

	Intent intent = new Intent(ActivityA.this,ActivityB.class);
	intent.putExtras("list",(Serializable)list);
	startActivity(intent);
```

**ActivityB** 中获取数据：

```
Intent intent = getIntent(); 
List<User> list = (List<User>) getIntent().getSerializableExtra("list");
```


### <span style="color:#0000ff;">2.2 传递 Map 集合数据</span>

Map接口及他的实现类默认是没有实现序列化的接口的，我们要想传递 **Map** 就要让 **Map** 实现序列化接口，我们可以自定义一个类，以HashMap为例吧，我们的类就叫 **SerializableHashMap** 吧，然后让定义一个 **Map** 变量作为成员属性并实现序列化接口，这样我们的类就可以进行传递了，**SerializableHashMap** 的实现如下：

```
public class SerializableHashMap implements Serializable {

    private HashMap<String, String> map;

    public SerializableHashMap() {
    }

    public SerializableHashMap(HashMap<String, String> map) {
        this.map = map;
    }

    public HashMap<String, String> getMap() {
        return map;
    }

    public void setMap(HashMap<String, String> map) {
        this.map = map;
    }

}
```

代码示例：

**ActivityA** 中设置数据：

```
	HashMap<String, Object> map = new HashMap<>();
    map.put("name", "Jack");
    map.put("age", 18);

	SerializableHashMap sMap = new SerializableHashMap();
    sMap.setMap(map); // 将map数据添加到封装的sMap中
    Bundle bundle = new Bundle();
    bundle.putSerializable("map", sMap);
    Intent intent = new Intent(ActivityA.this,ActivityB.class);
    intent.putExtras(bundle);
	startActivity(intent);
```

**ActivityB** 中获取数据：

```
	Intent intent = getIntent(); 
    Bundle bundle = intent.getExtras();
    SerializableHashMap sMap = (SerializableHashMap) bundle.get("map");
    HashMap<String, Object> map = sMap.getMap();
```

### <span style="color:#0000ff;">2.3 如何选择？</span>

另外，默认 **Intent** 帮我们实现了，可以支持传递 **String** 数组等，也比较简单，这里就不赘述了，另外如果数据量比较大的情况下，建议使用第三方框架来进行传递数据，例如：**EventBus** 等来代替，这样的话可以避免造成 **TransactionTooLargeException**。

如何选择哪种序列化方式？弄清楚他们的区别，你也就知道使用哪个更合适了。

**Serializable** 和 **Parcelable** 接口的区别：

- 在使用内存的时候，**Parcelable** 比 **Serializable** 性能高，所以推荐使用 **Parcelable**；
- **Serializable** 在序列化的时候会产生大量的临时变量，从而引起频繁的 **GC**；
- **Parcelable** 不能使用在要将数据存储在磁盘上的情况，因为 **Parcelable** 不能很好的保证数据的 持续性，在外界有变化的情况下，尽管 **Serializable** 效率低点，但此时还是建议使用**Serializable**；

好了，今天的分享就到这里，我还是你们的大飞哥，喜欢我的文章的就点个赞，点赞的人最可爱！

![](http://pcobk8jbf.bkt.clouddn.com/FjHrnzvoQFwJ0dOmvazQI0x9alHf)

**猜你喜欢：**  
[给大家推荐一个神器](https://mp.weixin.qq.com/s/ReJJFmQm6l55r9pvo7aFIA)  
[深入浅出Retrofit2.x(一)](https://mp.weixin.qq.com/s/jxBEg7dpFfTNLXNFdvJjrw)  
[MySQL8.0设置远程访问权限](https://mp.weixin.qq.com/s/3GlXPLakrVhuq74gOk1wmQ)  
[不得不会的10点Java基础知识](https://mp.weixin.qq.com/s/H8WqFy2RPYZWRpp63oBPfQ)  
[CentOS7 重置 MySQL 8.0 密码](https://mp.weixin.qq.com/s/EwD4Tp9zvaHFfZ17ayTksg)  

<center>扫一扫，开启我们的的缘分</center>
<center>![](http://pcobk8jbf.bkt.clouddn.com/FijAsROsRHuwVUivQ8M9D5o3RSdP)</center>
<center>可能不是最好的公众号，但肯定是最良心的公众号!</center>