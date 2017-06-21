#Android Picasso实现圆形图片和圆角图片

##1.实现圆形图片

###1.1代码调用如下

```
Picasso.with(mContext).load(headpic).memoryPolicy(MemoryPolicy.NO_CACHE)
                           .transform(new CircleTransform(mContext)).into(ivIcon);
```

###1.2自定义圆形图片处理工具类(可以修改圆形图片的半径)

```
import android.content.Context;
import android.graphics.Bitmap;
import android.graphics.BitmapShader;
import android.graphics.Canvas;
import android.graphics.Paint;

import com.squareup.picasso.Transformation;

/**
 * Created by xpf on 2017/4/8 :)
 * Function:Picasso Transformation 实现圆形图片
 */

public class CircleTransform implements Transformation {

    private Context mContext;
    private int radius; // 圆形图片半径

    public CircleTransform(Context context) {
        mContext = context;
        radius = DensityUtil.dp2px(mContext, 10);
    }

    @Override
    public Bitmap transform(Bitmap source) {

        int size = Math.min(source.getWidth(), source.getHeight());

        int x = (source.getWidth() - size) / 2;
        int y = (source.getHeight() - size) / 2;

        Bitmap squaredBitmap = Bitmap.createBitmap(source, x, y, size, size);
        if (squaredBitmap != source) {
            source.recycle();
        }

        Bitmap bitmap = Bitmap.createBitmap(size, size, source.getConfig());

        Canvas canvas = new Canvas(bitmap);
        Paint paint = new Paint();
        BitmapShader shader = new BitmapShader(squaredBitmap,
                BitmapShader.TileMode.CLAMP, BitmapShader.TileMode.CLAMP);
        paint.setShader(shader);
        paint.setAntiAlias(true);

        float r = size / 2f;
//        float r = radius;
        canvas.drawCircle(r, r, r, paint);

        squaredBitmap.recycle();
        return bitmap;
    }

    @Override
    public String key() {
        return "circle";
    }

}
```

##2.实现圆角图片

###2.1代码调用如下

```
Picasso.with(mContext).load(headpic).memoryPolicy(MemoryPolicy.NO_CACHE)
                           .transform(new RoundTransform(mContext)).into(ivIcon);
```

###2.2自定义圆角图片处理工具类(可以修改图片圆角的半径)

```
import android.content.Context;
import android.graphics.Bitmap;
import android.graphics.Canvas;
import android.graphics.Paint;
import android.graphics.PorterDuff;
import android.graphics.PorterDuffXfermode;
import android.graphics.Rect;
import android.graphics.RectF;

import com.squareup.picasso.Transformation;

/**
 * Created by xpf on 2017/4/8 :)
 * Function:Picasso Transformation 实现圆角图片
 */

public class RoundTransform implements Transformation {

    private Context mContext;

    public RoundTransform(Context context) {
        mContext = context;
    }

    @Override
    public Bitmap transform(Bitmap source) {

        int widthLight = source.getWidth();
        int heightLight = source.getHeight();
        int radius = DensityUtil.dp2px(mContext, 8); // 圆角半径

        Bitmap output = Bitmap.createBitmap(source.getWidth(), source.getHeight(), Bitmap.Config.ARGB_8888);

        Canvas canvas = new Canvas(output);
        Paint paintColor = new Paint();
        paintColor.setFlags(Paint.ANTI_ALIAS_FLAG);

        RectF rectF = new RectF(new Rect(0, 0, widthLight, heightLight));

        canvas.drawRoundRect(rectF, radius, radius, paintColor);
//        canvas.drawRoundRect(rectF, widthLight / 5, heightLight / 5, paintColor);

        Paint paintImage = new Paint();
        paintImage.setXfermode(new PorterDuffXfermode(PorterDuff.Mode.SRC_ATOP));
        canvas.drawBitmap(source, 0, 0, paintImage);
        source.recycle();
        return output;
    }

    @Override
    public String key() {
        return "roundcorner";
    }

}
```

今天就到这里吧...

下篇预告：Android Glide实现圆形图片和圆角图片

敬请期待...
