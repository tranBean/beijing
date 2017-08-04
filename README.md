package com.beansun.tranbean.myapplication;

import android.graphics.Color;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.text.Spannable;
import android.text.SpannableString;
import android.text.SpannableStringBuilder;
import android.text.style.AbsoluteSizeSpan;
import android.text.style.ForegroundColorSpan;
import android.view.View;
import android.widget.TextView;

import java.util.ArrayList;
import java.util.List;

public class MainActivity extends AppCompatActivity {
    private String string = "前日比率 (外貨)";
    private int x;
    private int y;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        StringBuffer sb = new StringBuffer();
        List list = new ArrayList<Integer>();
        TextView textView = (TextView) findViewById(R.id.text);
        for (int i = 0; i < string.length(); i++) {
            if (string.charAt(i) == '(') {
                x = i;
            }
            if (string.charAt(i) == ')') {
                y = i;
            }
        }
        SpannableStringBuilder style = new SpannableStringBuilder(string);
        style.setSpan(new AbsoluteSizeSpan(28), x, y + 1, Spannable.SPAN_EXCLUSIVE_EXCLUSIVE);
        //for (int i = 0; i < list.size(); i++) {
        //System.out.println(list.get(i)+", ");
//      style.setSpan(new BackgroundColorSpan(Color.RED),list.get(i),list.get(i)+1,Spannable.SPAN_EXCLUSIVE_INCLUSIVE);   //设置指定位置textview的背景颜色
        style.setSpan(new ForegroundColorSpan(Color.RED), x, y + 1, Spannable.SPAN_EXCLUSIVE_INCLUSIVE);   //设置指定位置文字的颜色
        //}
        textView.setText(style);

    }
}

http://blog.csdn.net/harvic880925/article/details/41553177


                dm = new DisplayMetrics();
                getWindowManager().getDefaultDisplay().getMetrics(dm);
                //获得手机的宽带和高度像素单位为px
                String str = "手机屏幕分辨率为:" + dm.widthPixels
                        +" * "+dm.heightPixels;
                textview1.setText(str);
