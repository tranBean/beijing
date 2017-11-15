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
                
                
                
                http://blog.csdn.net/harvic880925/article/details/51523983
                
 ドライバ 更新
Android Composite ADB Interfaceを認識し、ドライバのインストール画面が起動します。
※ドライバのインストール画面が起動しない場合、以下の操作をお試し下さい。
1. PCのコントロールパネル を開き、一覧から デバイスマネージャー を開きます。
2. [ほかのデバイス] - [Android ADB Interface] を右クリックし、[ドライバソフトウェアの更新] を選択します。
3. [コンピューターを参照してドライバソフトウェアを検索します] を選択します。
ドライバ格納先で"\usb_driver_SHARP"を選択し、ドライバをインストールします。

adb shell setprop log.tag.AppIndexApi VERBOSE
adb logcat -v time -s AppIndexApi:V


視聴中に画面をタップした際に下記の要素を表示すること。すべての要素は3秒無操作で消えること。
リニア放送中はNOW ON AIRタグを表示すること。
録画コンテンツ視聴中はシークバーを表示すること。
現在放送中番組はシーク不可とすること。
縦画面/横画面の切り替えができること。
全画面表示ボタンによる画面の切り替えを行う。

録画番組のみ操作アイコンを表示すること。表示要素は以下とする。
10秒戻し
再生/一時停止
30秒送り
