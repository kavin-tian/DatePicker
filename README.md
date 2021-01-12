Android 自定义日期选择控件

参考：https://github.com/liuwan1992/CustomDatePicker ，感谢！

依赖
```
allprojects {
    repositories {
        ...
        maven { url 'https://jitpack.io' }
    }
}
```

```
dependencies {
	        implementation 'com.github.kavin-tian:datepicker:1.1.0'
	}
```


使用
```
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        TextView datetime = findViewById(R.id.datetime);
        Button pickTime = findViewById(R.id.pick_time);
        pickTime.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                getTimerPicker(datetime, "").show(System.currentTimeMillis());
            }
        });
    }

    /**
     * 初始化时间选择器
     */
    private CustomDatePicker getTimerPicker(TextView tv_time, String pattern) {
        String beginTime = "2018-10-01 00:00";
        String endTime = "2025-10-01 00:00";

        // 通过日期字符串初始化日期，格式请用：yyyy-MM-dd HH:mm
        CustomDatePicker mTimerPicker = new CustomDatePicker(this, new CustomDatePicker.Callback() {
            @Override
            public void onTimeSelected(long timestamp) {
                String datetime;
                if (TextUtils.isEmpty(pattern)) {
                    datetime = timeStamp2date(timestamp, pattern_default);
                } else {
                    datetime = timeStamp2date(timestamp, pattern);
                }
                tv_time.setText(datetime);

            }
        }, beginTime, endTime);
        // 允许点击屏幕或物理返回键关闭
        mTimerPicker.setCancelable(true);
        // 显示时和分
        mTimerPicker.setCanShowPreciseTime(true);
        // 允许循环滚动
        mTimerPicker.setScrollLoop(true);
        // 允许滚动动画
        mTimerPicker.setCanShowAnim(true);
        // 隐藏日期,仅显示时间
        //mTimerPicker.hideDate();

        return mTimerPicker;

    }

    public static final String pattern_default = "yyyy-MM-dd HH:mm:ss";

    /**
     * 时间戳转字符串
     */
    public static String timeStamp2date(long timestamp, String pattern) {
        return new SimpleDateFormat(pattern, Locale.CHINA).format(new Date(timestamp));
    }
}
```