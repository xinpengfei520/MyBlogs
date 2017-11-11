
#Android实现密码隐藏显示

需求：当用户点击提现按钮的时候，弹出输入支付密码的popupwindow,输入密码框后面有显示和隐藏支付密码的切换按钮。

老规矩先验货：



##1.布局文件

```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout
        android:layout_width="275dp"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:background="#FFFFFF"
        android:orientation="vertical">

        <TextView
            android:id="@+id/tvRecharge"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="16dp"
            android:gravity="center"
            android:text="支付密码验证"
            android:textSize="18sp" />

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_margin="10dp"
            android:background="#11000000"
            android:gravity="center_vertical"
            android:orientation="horizontal">

            <EditText
                android:id="@+id/etPayPwd"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:background="@null"
                android:hint="输入支付密码"
                android:inputType="textPassword"
                android:padding="10dp"
                android:paddingRight="10dp" />

            <ToggleButton
                android:id="@+id/togglePwd"
                android:layout_width="20dp"
                android:layout_height="20dp"
                android:layout_marginRight="10dp"
                android:background="@drawable/toggle_pwd_selector"
                android:padding="10dp"
                android:textOff=""
                android:textOn="" />
        </LinearLayout>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal">

            <TextView
                android:id="@+id/tvCancel"
                android:layout_width="0dp"
                android:layout_height="48dp"
                android:layout_weight="1"
                android:gravity="center"
                android:text="取消"
                android:textSize="16sp" />

            <TextView
                android:id="@+id/tvConfirm"
                android:layout_width="0dp"
                android:layout_height="48dp"
                android:layout_weight="1"
                android:gravity="center"
                android:text="确定"
                android:textSize="16sp" />
        </LinearLayout>
    </LinearLayout>

</RelativeLayout>
```

##2.代码实现

```
public class MainActivity extends AppCompatActivity {

    private PopupWindow popupWindow;
    private View popupView;
    private EditText etPayPwd;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button btnWithdraw = (Button) findViewById(R.id.btnWithdraw);
        btnWithdraw.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                showPayPwdDialog();
                BrightnessManager.lightOff(MainActivity.this);
            }
        });
    }

    private void showPayPwdDialog() {
        if (popupWindow == null) {
            popupView = View.inflate(MainActivity.this, R.layout.popupwindow_pay_pwd, null);
            popupWindow = new PopupWindow(popupView, WindowManager.LayoutParams.MATCH_PARENT,
                    WindowManager.LayoutParams.MATCH_PARENT);
            popupWindow.setOnDismissListener(new PopupWindow.OnDismissListener() {
                @Override
                public void onDismiss() {
                    BrightnessManager.lightOn(MainActivity.this);
                    etPayPwd.setText("");
                }
            });
            popupWindow.setBackgroundDrawable(new BitmapDrawable());
            popupWindow.setFocusable(true);
            popupWindow.setOutsideTouchable(true);

            ToggleButton togglePwd = (ToggleButton) popupView.findViewById(R.id.togglePwd);
            etPayPwd = (EditText) popupView.findViewById(R.id.etPayPwd);
            togglePwd.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
                @Override
                public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
                    if (isChecked) {
                        //如果选中，显示密码
                        etPayPwd.setTransformationMethod(HideReturnsTransformationMethod.getInstance());
                    } else {
                        //否则隐藏密码
                        etPayPwd.setTransformationMethod(PasswordTransformationMethod.getInstance());
                    }
                }
            });

            popupView.findViewById(R.id.tvCancel).setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    popupWindow.dismiss();
                    BrightnessManager.lightOn(MainActivity.this);
                }
            });

            popupView.findViewById(R.id.tvConfirm).setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    String payPwd = etPayPwd.getText().toString();
                    popupWindow.dismiss();
                    BrightnessManager.lightOn(MainActivity.this);
                    if (!TextUtils.isEmpty(payPwd)) {
                        Toast.makeText(MainActivity.this, "提现请求提交成功！", Toast.LENGTH_SHORT).show();
                    }
                }
            });
        }

        if (popupWindow.isShowing()) {
            popupWindow.dismiss();
            BrightnessManager.lightOn(MainActivity.this);
        }
        popupWindow.showAtLocation(MainActivity.this.findViewById(R.id.activity_main),
                Gravity.BOTTOM | Gravity.CENTER_HORIZONTAL, 0, 0);
    }
}
```

OK！今天双11，马云硬是将光棍节过成了购物狂欢节，你让我们这些光棍儿们怎么想？好吧，与我们没关系，咱们继续撸咱们的代码，为了早日迎娶白富美，走向人生巅峰，继续撸不要停！！！