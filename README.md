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


public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        VideoView videoView = (VideoView) this.findViewById(R.id.vv_video);

        videoView.setVideoURI(Uri.parse("http://192.168.128.190:8080/carlock.mp4"));

        MediaController ctr = new MediaController(this);

        videoView.setMediaController(ctr);

        ctr.setMediaPlayer(videoView);
    }
    
    <?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <VideoView
        android:id="@+id/vv_video"
        android:layout_alignParentTop="true"
        android:layout_centerInParent="true"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>

</RelativeLayout>


---1121
package com.awesome.tranbean.videodemomooc;

import android.content.res.Configuration;
import android.media.AudioManager;
import android.net.Uri;
import android.os.Handler;
import android.os.Message;
import android.support.v4.app.FragmentActivity;
import android.os.Bundle;
import android.view.MotionEvent;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ImageView;
import android.widget.MediaController;
import android.widget.RelativeLayout;
import android.widget.SeekBar;
import android.widget.TextView;
import android.widget.VideoView;

public class MainActivity extends FragmentActivity {

    private static final int UPDATE_UI = 0;
    private VideoView mVideoView;
    private View ctrlBar;
    private SeekBar progressSeekBar;
    private ImageView playCtrl;
    private TextView curtime;
    private TextView tottime;
    private ImageView fullScreen;
    private SeekBar audioSb;
    private ImageView audio;
    private int screentW;
    private int screentH;
    private RelativeLayout videoLayout;
    private AudioManager mAudioManager;

    private boolean isAdjust = false;
    private int threshold = 54;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mAudioManager = (AudioManager) getSystemService(AUDIO_SERVICE);
        initView();
        mVideoView = findViewById(R.id.main_vv);

        //mVideoView.setVideoPath("");

        mVideoView.setVideoURI(Uri.parse("http://192.168.128.190:8080/carlock.mp4"));

        MediaController meCtrl = new MediaController(this);
        mVideoView.setMediaController(meCtrl);
        meCtrl.setMediaPlayer(mVideoView);

        setPlayerEvent();

        mVideoView.start();
        UIHandler.sendEmptyMessage(UPDATE_UI);


    }

    private void updateTextViewWithTimeFormat(TextView tv, int millsec) {
        int second = millsec / 1000;
        int hh = second / 3600;
        int mm = second % 3600 / 60;
        int ss = second % 60;

        String str = null;
        if (hh != 0) {
            //至少两位10位数整数
            str = String.format("%02d:%02d:%02d", hh, mm, ss);
        } else {
            str = String.format("%02d:%02d", mm, ss);
        }
        tv.setText(str);
    }


    private Handler UIHandler = new Handler() {
        @Override
        public void handleMessage(Message msg) {
            super.handleMessage(msg);

            if (msg.what == UPDATE_UI) {

                int currentPosition = mVideoView.getCurrentPosition();
                int totalduration = mVideoView.getDuration();

                //
                updateTextViewWithTimeFormat(curtime, currentPosition);
                updateTextViewWithTimeFormat(tottime, totalduration);
                //
                progressSeekBar.setMax(totalduration);
                progressSeekBar.setProgress(currentPosition);

                //
                UIHandler.sendEmptyMessageDelayed(UPDATE_UI, 500);
            }


        }
    };

    private void setPlayerEvent() {
        playCtrl.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                if (mVideoView.isPlaying()) {
                    //playCtrl.setImageResource(R.drawable.play_btn_style);
                    //
                    mVideoView.pause();
                    //停止有爱刷新
                    UIHandler.removeMessages(UPDATE_UI);
                } else {

                    //
                    mVideoView.start();
                    //huifu有爱刷新
                    UIHandler.sendEmptyMessage(UPDATE_UI);
                }
            }
        });

        progressSeekBar.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {
            @Override
            public void onProgressChanged(SeekBar seekBar, int i, boolean b) {
                updateTextViewWithTimeFormat(curtime, i);
            }

            @Override
            public void onStartTrackingTouch(SeekBar seekBar) {
                UIHandler.removeMessages(UPDATE_UI);
            }

            @Override
            public void onStopTrackingTouch(SeekBar seekBar) {
                int progress = seekBar.getProgress();
                //视频进度到停止拖动到位置
                mVideoView.seekTo(progress);
                UIHandler.sendEmptyMessage(UPDATE_UI);
            }
        });

        audioSb.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {
            @Override
            public void onProgressChanged(SeekBar seekBar, int i, boolean b) {
                mAudioManager.setStreamVolume(AudioManager.STREAM_MUSIC, i, 0);
            }

            @Override
            public void onStartTrackingTouch(SeekBar seekBar) {
            }

            @Override
            public void onStopTrackingTouch(SeekBar seekBar) {
            }
        });

        mVideoView.setOnTouchListener(new View.OnTouchListener() {
            @Override
            public boolean onTouch(View view, MotionEvent motionEvent) {
                float x = motionEvent.getX();
                float y = motionEvent.getY();
                float lastX = 0;
                float lastY = 0;
                switch (motionEvent.getAction()) {
                    case MotionEvent.ACTION_DOWN: {
                        lastX = x;
                        lastY = y;
                    }
                    case MotionEvent.ACTION_MOVE: {
                        float detlaX = x - lastX;
                        float detlaY = y - lastY;
                        //偏移量绝对值
                        float absdetLaX = Math.abs(detlaX);
                        float absdetLaY = Math.abs(detlaY);
                        
                        if(absdetLaX > threshold && absdetLaY> threshold){
                            if(absdetLaX < absdetLaY){
                                isAdjust = true;
                            }else {
                                isAdjust = false;
                            }
                        }else if(absdetLaX<threshold && absdetLaY > threshold){
                            isAdjust = true;
                        }else if(absdetLaX>threshold && absdetLaY < threshold){
                            isAdjust = false;
                        }
                        
                        if(isAdjust){
                            if (x < screentW /2) {
                                if(detlaY>0){
                                    //降低亮度
                                }
                            }else {
                                //身高亮度
                            }
                        }else {
                            //调节声音
                            if(detlaY>0){
                                //jianxiao shenying
                            }else {
                                //tigao shengyin
                            }
                        }
                        lastX = x;
                        lastY = y;
                        break;
                    }
                    case MotionEvent.ACTION_UP: {

                    }

                }

                return true;
            }
        });
    }

    @Override
    protected void onPause() {
        super.onPause();
        //停止有爱刷新
        UIHandler.removeMessages(UPDATE_UI);

    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        //停止有爱刷新
        UIHandler.removeMessages(UPDATE_UI);

    }

    private void initView() {
        ctrlBar = findViewById(R.id.ctrl_bar_ll);
        progressSeekBar = findViewById(R.id.progress_seek_bar_sb);
        playCtrl = findViewById(R.id.pause_iv);
        curtime = findViewById(R.id.time_cur_tv);
        tottime = findViewById(R.id.time_total_tv);
        fullScreen = findViewById(R.id.screen_iv);
        audioSb = findViewById(R.id.audio_seek_bar_sb);
        audio = findViewById(R.id.audio_icon);
        videoLayout = findViewById(R.id.video_layout_rl);

        screentW = getResources().getDisplayMetrics().widthPixels;
        screentH = getResources().getDisplayMetrics().heightPixels;

        int streamMaxVolume = mAudioManager.getStreamMaxVolume(AudioManager.STREAM_MUSIC);
        int curVolume = mAudioManager.getStreamVolume(AudioManager.STREAM_MUSIC);
        audioSb.setMax(streamMaxVolume);
        audioSb.setProgress(curVolume);
    }

    @Override
    public void onConfigurationChanged(Configuration newConfig) {
        super.onConfigurationChanged(newConfig);
        if (getResources().getConfiguration().orientation == Configuration.ORIENTATION_LANDSCAPE) {

            //setViewPlayerScale(ViewGroup.LayoutParams.MATCH_PARENT,ViewGroup.LayoutParams.MATCH_PARENT);
            //setViewPlayerScale(screentW,screentH);
            audioSb.setVisibility(View.VISIBLE);
        } else {
            //竖到 240 px to dp必要！
            setViewPlayerScale(ViewGroup.LayoutParams.MATCH_PARENT, 240);
        }
    }

    private void setViewPlayerScale(int screentW, int screentH) {
        ViewGroup.LayoutParams layoutParams = mVideoView.getLayoutParams();
        layoutParams.height = screentH;
        layoutParams.width = screentW;
        mVideoView.setLayoutParams(layoutParams);

        ViewGroup.LayoutParams layoutParams1 = videoLayout.getLayoutParams();
        layoutParams1.height = screentH;
        layoutParams1.width = screentW;
        videoLayout.setLayoutParams(layoutParams1);

    }
}
---xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
	xmlns:android="http://schemas.android.com/apk/res/android"
	android:layout_width="match_parent"
	android:layout_height="match_parent">

	<RelativeLayout
		android:id="@+id/video_layout_rl"
		android:layout_width="match_parent"
		android:layout_height="240dp">

		<VideoView
			android:id="@+id/main_vv"
			android:layout_width="match_parent"
			android:layout_height="240dp"/>

		<LinearLayout
			android:id="@+id/ctrl_bar_ll"
			android:layout_width="match_parent"
			android:layout_height="50dp"
			android:layout_alignParentBottom="true"
			android:orientation="vertical">

			<!-- progressDrawable= selector -->
			<SeekBar
				android:id="@+id/progress_seek_bar_sb"
				android:layout_width="match_parent"
				android:layout_height="5dp"
				android:layout_marginLeft="-20dp"
				android:layout_marginRight="-20dp"
				android:indeterminate="false"
				android:max="100"
				android:progress="30"
				android:progressDrawable="@drawable/seekbar_style2"
				android:thumb="@null"/>

			<RelativeLayout
				android:layout_width="match_parent"
				android:layout_height="match_parent"
				android:background="#101010"
				android:gravity="center_vertical">

				<LinearLayout
					android:id="@+id/left_layout"
					android:layout_width="wrap_content"
					android:layout_height="match_parent"
					android:gravity="center_vertical"
					android:orientation="horizontal">

					<ImageView
						android:id="@+id/pause_iv"
						android:layout_width="wrap_content"
						android:layout_height="wrap_content"
						android:layout_marginLeft="16dp"
						android:src="@android:drawable/ic_menu_info_details"/>

					<TextView
						android:id="@+id/time_cur_tv"
						android:layout_width="wrap_content"
						android:layout_height="wrap_content"
						android:layout_marginLeft="32dp"
						android:text="00:00:00"
						android:textColor="@android:color/white"
						android:textSize="14dp"/>

					<TextView
						android:layout_width="wrap_content"
						android:layout_height="wrap_content"
						android:layout_marginLeft="5dp"
						android:text="/"
						android:textColor="#4c4c4c"
						android:textSize="14dp"/>

					<TextView
						android:id="@+id/time_total_tv"
						android:layout_width="wrap_content"
						android:layout_height="wrap_content"
						android:layout_marginLeft="5dp"
						android:text="00:00:00"
						android:textColor="@android:color/white"
						android:textSize="14dp"/>
				</LinearLayout>

				<LinearLayout
					android:layout_width="10dp"
					android:layout_height="match_parent"
					android:layout_alignParentRight="true"
					android:layout_toRightOf="@id/left_layout"
					android:gravity="center"
					android:orientation="horizontal">

					<ImageView
						android:id="@+id/audio_icon"
						android:layout_width="wrap_content"
						android:layout_height="wrap_content"
						android:src="@android:drawable/ic_menu_info_details"
						/>

					<SeekBar
						android:id="@+id/audio_seek_bar_sb"
						android:layout_width="100dp"
						android:layout_height="5dp"
						android:indeterminate="false"
						android:max="100"
						android:progress="20"
						android:progressDrawable="@drawable/seekbar_style2"
						android:thumb="@null"
						android:visibility="gone"/>

					<View
						android:layout_width="1dp"
						android:layout_height="match_parent"
						android:layout_marginBottom="5dp"
						android:layout_marginLeft="32dp"
						android:layout_marginTop="5dp"
						android:background="#1e1e1e"/>

					<ImageView
						android:id="@+id/screen_iv"
						android:layout_width="wrap_content"
						android:layout_height="wrap_content"
						android:layout_marginLeft="16dp"
						android:layout_marginRight="16dp"
						android:src="@android:drawable/ic_menu_info_details"/>
				</LinearLayout>
			</RelativeLayout>
		</LinearLayout>
	</RelativeLayout>

</RelativeLayout>
---1121 mediaplay surfaceview
package com.itheima.videoplayer;

import java.io.IOException;

import android.media.MediaPlayer;
import android.os.Bundle;
import android.app.Activity;
import android.view.Menu;
import android.view.SurfaceHolder;
import android.view.SurfaceHolder.Callback;
import android.view.SurfaceView;

public class MainActivity extends Activity {
	private MediaPlayer player;
	static int currentPosition;
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		
		SurfaceView sv = (SurfaceView) findViewById(R.id.sv);
		//拿到surfaceview的控制器
		final SurfaceHolder sh = sv.getHolder();
		
//		Thread t = new Thread(){
//			@Override
//			public void run() {
//				// TODO Auto-generated method stub
//				try {
//					sleep(200);
//				} catch (InterruptedException e) {
//					// TODO Auto-generated catch block
//					e.printStackTrace();
//				}
//				runOnUiThread(new Runnable() {
//					@Override
//					public void run() {
//						MediaPlayer player = new MediaPlayer();
//						player.reset();
//						try {
//							player.setDataSource("sdcard/2.3gp");
//							player.setDisplay(sh);
//							player.prepare();
//							player.start();
//						} catch (Exception e) {
//							// TODO Auto-generated catch block
//							e.printStackTrace();
//						} 
//						
//					}
//				});
//				
//			}
//		};
//		t.start();
		
		sh.addCallback(new Callback() {
			
			
			//surfaceView销毁时调用
			@Override
			public void surfaceDestroyed(SurfaceHolder holder) {
				//每次surfaceview销毁时，同时停止播放视频
				if(player != null){
					currentPosition = player.getCurrentPosition();
					player.stop();
					player.release();
					player = null;
				}
				
			}
			//surfaceView创建时调用
			@Override
			public void surfaceCreated(SurfaceHolder holder) {
				//每次surfaceView创建时才去播放视频
				if(player == null){
					player = new MediaPlayer();
					player.reset();
					try {
						player.setDataSource("sdcard/2.3gp");
						player.setDisplay(sh);
						player.prepare();
						player.start();
						player.seekTo(currentPosition);
					} catch (Exception e) {
						// TODO Auto-generated catch block
						e.printStackTrace();
					} 
				}
				
			}
			//surfaceView结构改变时调用
			@Override
			public void surfaceChanged(SurfaceHolder holder, int format, int width,
					int height) {
				// TODO Auto-generated method stub
				
			}
		});
		
	}


}

<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">

	<item android:id="@android:id/background">
		<shape>
			<solid android:color="#707070">

			</solid>
			<size android:height="5dp"/>
		</shape>
	</item>

	<item android:id="@android:id/progress">
		<clip>
			<shape>
				<solid android:color="#B94310"/>
				<size android:height="5dp"/>
			</shape>
		</clip>
	</item>
</layer-list>



private Handler mHandler = new Handler(Looper.getMainLooper());


mRootView.setOnClickListener(new View.OnClickListener(){
	@Override
	public void onClick(View view){
		//○１
		if(mBottomLayout.getVisibility() == View.VISIBLE
			|| mTopLayout.getVisibility == View.VISIBLE}}){
			mBottomLayout.setVisibility(View.GONE);
			mTopLayout.setVisibility(View.GONE);
		}
		//○２
		mBottomLayout.setVisibility(View.VISIBLE);
		mTopLayout.setVisibility(View.VISIBLE);
		//○3
		mHandler.postDelayed(new Runable(){
			@Override
			public void run() {
				mBottomLayout.setVisibility(View.GONE);
				mTopLayout.setVisibility(View.GONE);
			}
		} 3000);


	}
});
