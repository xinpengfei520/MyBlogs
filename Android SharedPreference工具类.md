#Android SharedPreferences工具类

###新建一个SpUtil工具类

```
/**
 * Created by xpf on 2017/03/25 :)
 * Function: sp存储的工具类
 */
public class SpUtil {

    private static final String ANLEKE = "anleke";

    private SpUtil() {
    }

    private static SpUtil instace = new SpUtil();
    private static SharedPreferences mSp = null;

    public static SpUtil getInstace() {

        if (mSp == null) {
            mSp = MyApplication.getContext().getSharedPreferences(ANLEKE, Context.MODE_PRIVATE);
        }
        return instace;
    }

    /**
     * 保存数据
     *
     * @param key   键
     * @param value 值
     */
    public void save(String key, Object value) {

        if (value instanceof String) {
            mSp.edit().putString(key, (String) value).commit();
        } else if (value instanceof Boolean) {
            mSp.edit().putBoolean(key, (Boolean) value).commit();
        } else if (value instanceof Integer) {
            mSp.edit().putInt(key, (Integer) value).commit();
        }
    }

    // 读取String类型数据
    public String getString(String key, String defValue) {
        return mSp.getString(key, defValue);
    }

    // 读取boolean类型数据
    public boolean getBoolean(String key, boolean defValue) {
        return mSp.getBoolean(key, defValue);
    }

    // 读取int类型数据
    public int getInt(String key, int defValue) {
        return mSp.getInt(key, defValue);
    }

}
```

###注：在MyApplication中获取上下文

```
	private static Context mContext;

    @Override
    public void onCreate() {
        super.onCreate();
        mContext = this;
    }

    // 获取全局上下文
    public static Context getContext() {
        return mContext;
    }
```