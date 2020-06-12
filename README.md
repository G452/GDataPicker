# GDataPicker
滚动时间选择器

一、集成步骤
1、项目的build中添加

  allprojects {
    repositories {
        ……
        ……
        maven { url 'https://jitpack.io' }
    }
}

2、主moudel的build中添加

  implementation 'com.github.G452:GDataPicker:0.0.2'
  implementation 'com.github.G452:GDataPicker:0.0.3'//AndroidX用户
  
3、点击Sync Now刷新编译

二、使用步骤

1、布局中添加
  
  1）、日期选择器
 
  <TextView
        android:layout_width="match_parent"
        android:layout_height="50dp"
        android:background="@null"
        android:gravity="center_vertical"
        android:paddingStart="15dp"
        android:text="@string/select_date"
        android:textColor="@color/select_title_text"
        android:textSize="15sp" />

    <LinearLayout
        android:id="@+id/ll_date"
        android:layout_width="match_parent"
        android:layout_height="50dp"
        android:background="@color/current_time_bg"
        android:gravity="center_vertical"
        android:orientation="horizontal">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:layout_marginStart="15dp"
            android:background="@null"
            android:gravity="center_vertical"
            android:text="@string/current_date"
            android:textColor="@color/current_time_text"
            android:textSize="15sp" />

        <Space
            android:layout_width="0dp"
            android:layout_height="match_parent"
            android:layout_weight="1" />

        <TextView
            android:id="@+id/tv_selected_date"
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:layout_marginEnd="15dp"
            android:background="@null"
            android:gravity="center_vertical"
            android:textColor="@color/selected_time_text"
            android:textSize="15sp" />

    </LinearLayout>
     
     
 2）、时间选择器
    
      <TextView
        android:layout_width="match_parent"
        android:layout_height="50dp"
        android:background="@null"
        android:gravity="center_vertical"
        android:paddingStart="15dp"
        android:text="@string/select_time"
        android:textColor="@color/select_title_text"
        android:textSize="15sp" />

    <LinearLayout
        android:id="@+id/ll_time"
        android:layout_width="match_parent"
        android:layout_height="50dp"
        android:background="@color/current_time_bg"
        android:gravity="center_vertical"
        android:orientation="horizontal">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:layout_marginStart="15dp"
            android:background="@null"
            android:gravity="center_vertical"
            android:text="@string/current_time"
            android:textColor="@color/current_time_text"
            android:textSize="15sp" />

        <Space
            android:layout_width="0dp"
            android:layout_height="match_parent"
            android:layout_weight="1" />

        <TextView
            android:id="@+id/tv_selected_time"
            android:layout_width="wrap_content"
            android:layout_height="match_parent"
            android:layout_marginEnd="15dp"
            android:background="@null"
            android:gravity="center_vertical"
            android:textColor="@color/selected_time_text"
            android:textSize="15sp" />

    </LinearLayout>
    
    
2、Activity或Fragment中
    
1）、初始化日期选择器
        
        findViewById(com.g2452.www.gdatepickerlib.R.id.ll_date).setOnClickListener(this);
        mTvSelectedDate = findViewById(com.g2452.www.gdatepickerlib.R.id.tv_selected_date);
        initDatePicker();
        
 2）、初始化日期选择器
        
        findViewById(com.g2452.www.gdatepickerlib.R.id.ll_time).setOnClickListener(this);
        mTvSelectedTime = findViewById(com.g2452.www.gdatepickerlib.R.id.tv_selected_time);
        initTimerPicker();
    
3、初始化方法initDatePicker()和initTimerPicker()

        private void initDatePicker() {
        long beginTimestamp = DateFormatUtils.str2Long("2009-05-01", false);
        long endTimestamp = System.currentTimeMillis();

        mTvSelectedDate.setText(DateFormatUtils.long2Str(endTimestamp, false));

        // 通过时间戳初始化日期，毫秒级别
        mDatePicker = new CustomDatePicker(this, new CustomDatePicker.Callback() {
            @Override
            public void onTimeSelected(long timestamp) {
                mTvSelectedDate.setText(DateFormatUtils.long2Str(timestamp, false));
            }
        }, beginTimestamp, endTimestamp);
        // 不允许点击屏幕或物理返回键关闭
        mDatePicker.setCancelable(false);
        // 不显示时和分
        mDatePicker.setCanShowPreciseTime(false);
        // 不允许循环滚动
        mDatePicker.setScrollLoop(false);
        // 不允许滚动动画
        mDatePicker.setCanShowAnim(false);
    }
    
        private void initTimerPicker() {
        String beginTime = "2018-10-17 18:00";
        String endTime = DateFormatUtils.long2Str(System.currentTimeMillis(), true);

        mTvSelectedTime.setText(endTime);

        // 通过日期字符串初始化日期，格式请用：yyyy-MM-dd HH:mm
        mTimerPicker = new CustomDatePicker(this, new CustomDatePicker.Callback() {
            @Override
            public void onTimeSelected(long timestamp) {
                mTvSelectedTime.setText(DateFormatUtils.long2Str(timestamp, true));
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
    }
    
    
4、点击事件

    @Override
    public void onClick(View v) {
        switch (v.getId()) {
            case com.g2452.www.gdatepickerlib.R.id.ll_date:
                // 日期格式为yyyy-MM-dd
                mDatePicker.show(mTvSelectedDate.getText().toString());
                break;
            case com.g2452.www.gdatepickerlib.R.id.ll_time:
                // 日期格式为yyyy-MM-dd HH:mm
                mTimerPicker.show(mTvSelectedTime.getText().toString());
                break;
        }
    }
    
    
5、最后记得页面销毁时同时销毁View


      @Override
      protected void onDestroy() {
        super.onDestroy();
        mDatePicker.onDestroy();
    }

    
  
