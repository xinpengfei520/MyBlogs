#Android设置屏幕亮度及Alpha值

```
package com.xpf.someexamples;

import android.app.Activity;
import android.content.Context;
import android.view.Window;
import android.view.WindowManager;

/**
 * Created by xpf on 2017/5/14 :)
 * GitHub:xinpengfei520
 * Function:设置屏幕亮度及Alpha值
 */

public class BrightnessManager {
    /**
     * 设置当前activity的屏幕亮度
     *
     * @param paramFloat 0-1.0f
     * @param context    需要调整亮度的activity context
     */
    public static void setBrightness(float paramFloat, Context context) {
        Activity activity = (Activity) context;
        Window localWindow = activity.getWindow();
        WindowManager.LayoutParams params = localWindow.getAttributes();
        params.screenBrightness = paramFloat;
        localWindow.setAttributes(params);
    }

    /**
     * 获取当前activity的屏幕亮度
     *
     * @param context 当前activity context对象
     * @return 亮度值范围为0-0.1f，如果为-1.0，则亮度与全局同步
     */
    public static float getBrightness(Context context) {
        Activity activity = (Activity) context;
        Window localWindow = activity.getWindow();
        WindowManager.LayoutParams params = localWindow.getAttributes();
        return params.screenBrightness;
    }

    /**
     * 设置手机屏幕透明度0-1.0f
     */
    public static void setAlpha(float light, Context context) {
        Activity activity = (Activity) context;
        Window window = activity.getWindow();
        WindowManager.LayoutParams lp = window.getAttributes();
        lp.alpha = light;
        window.setAttributes(lp);
    }

    /**
     * 设置手机屏幕透明度变暗
     */
    public static void lightoff(Context context) {
        Activity activity = (Activity) context;
        Window window = activity.getWindow();
        WindowManager.LayoutParams lp = window.getAttributes();
        lp.alpha = 0.3f;
        window.setAttributes(lp);
    }

    /**
     * 设置手机屏幕透明度显示正常
     */
    public static void lighton(Context context) {
        Activity activity = (Activity) context;
        Window window = activity.getWindow();
        WindowManager.LayoutParams lp = window.getAttributes();
        lp.alpha = 1.0f;
        window.setAttributes(lp);
    }
}
```
