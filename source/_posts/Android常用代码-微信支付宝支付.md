---
title: Android常用代码-微信支付宝支付
date: 2017-10-17 08:47:51
tags:
	- Android
categories: Android常用代码
---

记录一下支付功能简单实现逻辑。

## 微信支付

### 注册一个入口Activity
```
<!--weixin pay-->
<activity
    android:name=".wxapi.WXPayEntryActivity"
    android:exported="true"
    android:launchMode="singleTop" />
<!--weixin pay-->

```

<!-- more -->

### 可自定义界面
```java
package com.keqiang.highcloud.wxapi;
  
import android.content.Intent;
import android.os.Bundle;
import android.widget.TextView;
  
import com.keqiang.highcloud.Constants;
import com.keqiang.highcloud.R;
import com.keqiang.highcloud.view.activity.BaseActivity;
import com.keqiang.highcloud.view.activity.shop.ShopMyOrderActivity;
import com.tencent.mm.opensdk.constants.ConstantsAPI;
import com.tencent.mm.opensdk.modelbase.BaseReq;
import com.tencent.mm.opensdk.modelbase.BaseResp;
import com.tencent.mm.opensdk.openapi.IWXAPI;
import com.tencent.mm.opensdk.openapi.IWXAPIEventHandler;
import com.tencent.mm.opensdk.openapi.WXAPIFactory;
  
public class WXPayEntryActivity extends BaseActivity implements IWXAPIEventHandler {
  
    private static final String TAG = "MicroMsg.SDKSample.WXPayEntryActivity";
    private IWXAPI api;
    private TextView tvResult;
  
    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
  
        api = WXAPIFactory.createWXAPI(this, Constants.APP_ID);
        api.handleIntent(getIntent(), this);
//        finish();
    }
  
    @Override
    public int getLayoutId() {
        return R.layout.pay_result;
    }
  
    @Override
    public void initView() {
        tvResult = (TextView) findViewById(R.id.tv_result);
    }
  
    @Override
    public void initData() {

    }
  
    @Override
    public void initEvent() {

    }
  
    @Override
    public void saveState(Bundle bundle) {

    }
  
    @Override
    public void restoreState(Bundle bundle) {

    }
  
    @Override
    public boolean isDefaultBackClose() {
        return false;
    }
  
    @Override
    protected void onNewIntent(Intent intent) {
        super.onNewIntent(intent);
        setIntent(intent);
        api.handleIntent(intent, this);
//        finish();
    }
  
    @Override
    public void onReq(BaseReq req) {
    }
  
    @Override
    public void onResp(BaseResp resp) {
  
        if (resp.getType() == ConstantsAPI.COMMAND_PAY_BY_WX) {
            if (resp.errCode == 0) {
                tvResult.setText("微信支付完成！");
                Intent intent = new Intent(WXPayEntryActivity.this, ShopMyOrderActivity.class);
                startActWithIntent(intent);
            } else {
                tvResult.setText(resp.errStr);
            }
            finish();
  
/*            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.setTitle(R.string.app_tip);
            builder.setMessage(getString(R.string.pay_result_callback_msg, String.valueOf(resp.errCode)));
            builder.show();*/
        }
    }
}
```

### 为保证支付前已将app注册到微信

- 注册一个广播

```xml
    <!-- wx pay start-->
    <receiver
        android:name=".other.receiver.AppRegister"
        android:permission="com.tencent.mm.plugin.permission.SEND" >
        <intent-filter>
            <action android:name="com.tencent.mm.plugin.openapi.Intent.ACTION_REFRESH_WXAPP" />
        </intent-filter>
    </receiver>
    <!--wx pay end-->
```

- 在Application中发送广播

```java
    /*注册APP到微信*/
    Intent intent = new Intent();
    intent.setAction("com.tencent.mm.plugin.openapi.Intent.ACTION_REFRESH_WXAPP");
    sendBroadcast(intent);

```

## 支付宝支付

### 网页登陆支付

```xml
    <!-- alipay sdk begin -->
    <activity
        android:name="com.alipay.sdk.app.H5PayActivity"
        android:configChanges="orientation|keyboardHidden|navigation|screenSize"
        android:exported="false"
        android:screenOrientation="behind"
        android:windowSoftInputMode="adjustResize|stateHidden"/>
    <activity
        android:name="com.alipay.sdk.app.H5AuthActivity"
        android:configChanges="orientation|keyboardHidden|navigation"
        android:exported="false"
        android:screenOrientation="behind"
        android:windowSoftInputMode="adjustResize|stateHidden"/>
    <!-- alipay sdk end -->
```

## 支付界面

### Layout

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/root"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
  
    <com.example.view.widget.ZzTitleBar
        android:id="@+id/title_bar"
        android:layout_width="match_parent"
        android:layout_height="@dimen/activity_title_height"
        android:background="@color/colorMain"
        app:leftImg="@drawable/iv_back"
        app:leftText="@string/back_text"
        app:showLeftLayout="true"
        app:showRightLayout="false"
        app:textColorAll="@color/color_white"
        app:textSizeAll="@dimen/activity_title_text_size"
        app:titleText="支付订单" />
  
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="88px"
        android:background="#fff"
        android:gravity="center"
        android:orientation="vertical">
  
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="支付剩余时间"
            android:textColor="#666"
            android:textSize="24px" />
  
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="4px"
            android:gravity="center_horizontal"
            android:orientation="horizontal">
  
            <TextView
                android:id="@+id/tv_minute_shi"
                android:layout_width="30px"
                android:layout_height="30px"
                android:background="@drawable/zhifu_top_kuang"
                android:gravity="center"
                android:text="0"
                android:textColor="#fff"
                android:textSize="20px" />
  
            <TextView
                android:id="@+id/tv_minute_ge"
                android:layout_width="30px"
                android:layout_height="30px"
                android:layout_marginLeft="10px"
                android:background="@drawable/zhifu_top_kuang"
                android:gravity="center"
                android:text="0"
                android:textColor="#fff"
                android:textSize="20px" />
  
            <TextView
                android:layout_width="30px"
                android:layout_height="30px"
                android:layout_marginLeft="5px"
                android:gravity="center"
                android:text=":"
                android:textColor="#333"
                android:textSize="20px" />
  
            <TextView
                android:id="@+id/tv_second_shi"
                android:layout_width="30px"
                android:layout_height="30px"
                android:layout_marginLeft="5px"
                android:background="@drawable/zhifu_top_kuang"
                android:gravity="center"
                android:text="0"
                android:textColor="#fff"
                android:textSize="20px" />
  
            <TextView
                android:id="@+id/tv_second_ge"
                android:layout_width="30px"
                android:layout_height="30px"
                android:layout_marginLeft="10px"
                android:background="@drawable/zhifu_top_kuang"
                android:gravity="center"
                android:text="0"
                android:textColor="#fff"
                android:textSize="20px" />
        </LinearLayout>
    </LinearLayout>
  
    <View
        android:layout_width="match_parent"
        android:layout_height="1px"
        android:background="#f1f1f1" />
  
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="112px"
        android:background="#fff"
        android:gravity="center_vertical"
        android:orientation="horizontal">
  
        <LinearLayout
            android:layout_width="260px"
            android:layout_height="match_parent"
            android:gravity="center_vertical"
            android:orientation="vertical">
  
            <TextView
                android:id="@+id/tv_product_name"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginLeft="80px"
                android:text="畅享管"
                android:textColor="#333"
                android:textSize="24px" />
  
            <LinearLayout
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginLeft="80px"
                android:layout_marginTop="4px"
                android:orientation="horizontal">
  
                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="数量："
                    android:textColor="#999"
                    android:textSize="18px" />
  
                <TextView
                    android:id="@+id/tv_number"
                    android:layout_width="100px"
                    android:layout_height="wrap_content"
                    android:background="@drawable/order_round_bg_shape"
                    android:gravity="center"
                    android:paddingBottom="2px"
                    android:paddingLeft="15px"
                    android:paddingRight="15px"
                    android:paddingTop="2px"
                    android:text="1"
                    android:textColor="#ff6631"
                    android:textSize="18px" />
            </LinearLayout>
  
            <LinearLayout
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginLeft="80px"
                android:layout_marginTop="4px"
                android:orientation="horizontal">
  
                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="型号："
                    android:textColor="#999"
                    android:textSize="18px" />
  
                <TextView
                    android:id="@+id/tv_type"
                    android:layout_width="100px"
                    android:layout_height="wrap_content"
                    android:background="@drawable/order_round_bg_shape"
                    android:gravity="center"
                    android:paddingBottom="2px"
                    android:paddingLeft="15px"
                    android:paddingRight="15px"
                    android:paddingTop="2px"
                    android:text=""
                    android:textColor="#ff6631"
                    android:textSize="18px" />
            </LinearLayout>
  
        </LinearLayout>
  
        <View
            android:layout_width="1px"
            android:layout_height="match_parent"
            android:layout_marginBottom="4px"
            android:layout_marginLeft="20px"
            android:layout_marginTop="4px"
            android:background="#f1f1f1" />
  
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginLeft="40px"
            android:gravity="bottom"
            android:orientation="horizontal">
  
            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="¥"
                android:textColor="#f74e37"
                android:textSize="20px" />
  
            <TextView
                android:id="@+id/tv_count"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:text=""
                android:textColor="#f74e37"
                android:textSize="60px" />
        </LinearLayout>
    </LinearLayout>
  
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="30px"
        android:background="#fff"
        android:orientation="vertical">
  
        <LinearLayout
            android:id="@+id/ll_alipay"
            android:layout_width="match_parent"
            android:layout_height="120px"
            android:background="@drawable/setting_item_bg_selector"
            android:clickable="true"
            android:gravity="center_vertical"
            android:orientation="horizontal">
  
            <ImageView
                android:layout_width="60px"
                android:layout_height="60px"
                android:layout_marginLeft="34px"
                android:src="@drawable/alipay_logo" />
  
            <LinearLayout
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_marginLeft="34px"
                android:layout_weight="1"
                android:orientation="vertical">
  
                <LinearLayout
                    android:layout_width="match_parent"
                    android:layout_height="wrap_content"
                    android:gravity="center_vertical"
                    android:orientation="horizontal">
  
                    <TextView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:text="支付宝"
                        android:textColor="#333"
                        android:textSize="30px" />
  
                    <ImageView
                        android:layout_width="60px"
                        android:layout_height="26px"
                        android:layout_marginLeft="20px"
                        android:src="@drawable/pay_type_recomend" />
                </LinearLayout>
  
                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_marginTop="10px"
                    android:text="亿万用户的选择 , 更快更安全"
                    android:textColor="#999"
                    android:textSize="20px" />
            </LinearLayout>
  
            <CheckBox
                android:id="@+id/cb_alipay"
                android:layout_width="34px"
                android:layout_height="34px"
                android:layout_marginRight="30px"
                android:background="@drawable/dingdan_btn_xuanzhe_selector"
                android:button="@null"
                android:checked="true"
                android:clickable="false"
                android:enabled="false" />
        </LinearLayout>
  
        <View
            android:layout_width="match_parent"
            android:layout_height="1px"
            android:background="#f1f1f1" />
  
        <LinearLayout
            android:id="@+id/ll_wxpay"
            android:layout_width="match_parent"
            android:layout_height="120px"
            android:background="@drawable/setting_item_bg_selector"
            android:clickable="true"
            android:gravity="center_vertical"
            android:orientation="horizontal">
  
            <ImageView
                android:layout_width="60px"
                android:layout_height="60px"
                android:layout_marginLeft="34px"
                android:src="@drawable/weixin_logo" />
  
            <LinearLayout
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_marginLeft="34px"
                android:layout_weight="1"
                android:orientation="vertical">
  
                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:text="微信"
                    android:textColor="#333"
                    android:textSize="30px" />
  
                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_marginTop="10px"
                    android:text="数亿用户都在用 , 安全可托付"
                    android:textColor="#999"
                    android:textSize="20px" />
            </LinearLayout>
  
            <CheckBox
                android:id="@+id/cb_weixin_pay"
                android:layout_width="34px"
                android:layout_height="34px"
                android:layout_marginRight="30px"
                android:background="@drawable/dingdan_btn_xuanzhe_selector"
                android:button="@null"
                android:checked="false"
                android:clickable="false"
                android:enabled="false" />
        </LinearLayout>
  
    </LinearLayout>
  
    <Button
        android:id="@+id/tv_pay"
        android:layout_width="match_parent"
        android:layout_height="110px"
        android:layout_marginLeft="30px"
        android:layout_marginRight="30px"
        android:layout_marginTop="140px"
        android:background="@drawable/btn_main_selector"
        android:gravity="center"
        android:text="确认付款"
        android:textColor="@color/color_white"
        android:textSize="36px" />
  
</LinearLayout>
```

### Java

```java
package com.keqiang.highcloud.view.activity.setting;
  
import android.annotation.SuppressLint;
import android.app.AlertDialog;
import android.content.DialogInterface;
import android.content.Intent;
import android.os.Bundle;
import android.os.Handler;
import android.os.Message;
import android.text.TextUtils;
import android.util.Log;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.CheckBox;
import android.widget.LinearLayout;
import android.widget.TextView;
import android.widget.Toast;
  
import com.alipay.sdk.app.EnvUtils;
import com.alipay.sdk.app.PayTask;
import com.ta.utdid2.android.utils.PhoneInfoUtils;
import com.tencent.mm.opensdk.modelpay.PayReq;
import com.tencent.mm.opensdk.openapi.IWXAPI;
import com.tencent.mm.opensdk.openapi.WXAPIFactory;
  
import org.apache.http.conn.util.InetAddressUtils;
import org.json.JSONObject;
  
import java.io.UnsupportedEncodingException;
import java.net.InetAddress;
import java.net.NetworkInterface;
import java.net.URLEncoder;
import java.util.ArrayList;
import java.util.Enumeration;
import java.util.Map;
import java.util.Timer;
import java.util.TimerTask;
  
import rx.Subscriber;
  
/**
 * 支付宝或微信支付
 * Created by zz on 2017/4/7.
 */
public class PayActivity extends BaseActivity {
  
    private TextView tvCount;
    private ZzTitleBar titleBar;
    private TextView tvPay;
  
    private static final int SDK_PAY_FLAG = 1;
    private static final int SDK_AUTH_FLAG = 2;
    private LinearLayout llAlipay;
    private LinearLayout llWxPay;
    private CheckBox cbAlipay;
    private CheckBox cbWxpay;
  
    //微信支付部分
    private IWXAPI api;
    private TextView tvMinteShi;
    private TextView tvMinuteGe;
    private TextView tvSecondShi;
    private TextView tvSecondGe;
    private String time;
  
    private Handler handler = new Handler();
    Runnable runnable = new Runnable() {
        @Override
        public void run() {
            runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    countTime(time);
                }
            });
        }
    };
    private String orderId;
    private TextView tvProductName;
    private TextView tvNumber;
    private TextView tvType;
    private String orderNo;
 
    @Override
    public int getLayoutId() {
        return R.layout.activity_alipay;
    }
  
    @SuppressLint("HandlerLeak")
    private Handler mHandler = new Handler() {
        @SuppressWarnings("unused")
        public void handleMessage(Message msg) {
            switch (msg.what) {
                case SDK_PAY_FLAG: {
                    @SuppressWarnings("unchecked")
                    PayResult payResult = new PayResult((Map<String, String>) msg.obj);
                    /**
                     对于支付结果，请商户依赖服务端的异步通知结果。同步通知结果，仅作为支付结束的通知。
                     */
                    String resultInfo = payResult.getResult();// 同步返回需要验证的信息
                    String resultStatus = payResult.getResultStatus();
                    // 判断resultStatus 为9000则代表支付成功
                    if (TextUtils.equals(resultStatus, "9000")) {
                        // 该笔订单是否真实支付成功，需要依赖服务端的异步通知。
                        ToastUtils.showCustomBgToast("支付成功！");
                        Intent intent = new Intent(PayActivity.this, ShopMyOrderActivity.class);
                        startActWithIntent(intent);
                        closeAct();
                    } else {
                        // 该笔订单真实的支付结果，需要依赖服务端的异步通知。
                        switch (resultStatus) {
                            case "6001":
                                ToastUtils.showCustomBgToast("已取消支付操作");
                                break;
                            default:
                                ToastUtils.showCustomBgToast("支付失败，错误代码："+resultStatus);
                                break;
                        }
                    }
                    break;
                }
                default:
                    break;
            }
        }
    };
  
    @Override
    public void initView() {
        titleBar = (ZzTitleBar) findViewById(R.id.title_bar);
        tvCount = (TextView) findViewById(R.id.tv_count);
        tvPay = (TextView) findViewById(R.id.tv_pay);
 
        tvType = (TextView) findViewById(R.id.tv_type);
 
        tvProductName = (TextView) findViewById(R.id.tv_product_name);
        tvNumber = (TextView) findViewById(R.id.tv_number);
  
        llAlipay = (LinearLayout) findViewById(R.id.ll_alipay);
        llWxPay = (LinearLayout) findViewById(R.id.ll_wxpay);
   
        cbAlipay = (CheckBox) findViewById(R.id.cb_alipay);
        cbWxpay = (CheckBox) findViewById(R.id.cb_weixin_pay);
  
        tvMinteShi = (TextView) findViewById(R.id.tv_minute_shi);
        tvMinuteGe = (TextView) findViewById(R.id.tv_minute_ge);
        tvSecondShi = (TextView) findViewById(R.id.tv_second_shi);
        tvSecondGe = (TextView) findViewById(R.id.tv_second_ge);
    }
  
    @Override
    public void initData() {
  
        //微信api
        api = WXAPIFactory.createWXAPI(this, Constants.APP_ID, false);
  
        orderId = getIntent().getStringExtra("orderId");
        orderNo = getIntent().getStringExtra("orderNo");
        String productName = getIntent().getStringExtra("productName");
        String total = getIntent().getStringExtra("total");
        String number = getIntent().getStringExtra("number");
        String model = getIntent().getStringExtra("model");
  
        tvType.setText(model);
        tvProductName.setText(productName);
        tvNumber.setText(number);
        tvCount.setText(total);
  
        String createTime = getIntent().getStringExtra("createTime");
        time = DateUtil.getTimeAddMinute(createTime, 16);
        if (time == null) {
            ToastUtils.showCustomBgToast("订单超时或已取消");
            return;
        }
        handler.postDelayed(runnable, 1000);
    }
  
  
    private void countTime(String time) {
        String curTime = DateUtil.getYearMonthDayHourMinuteSecond();
        long seconds = DateUtil.getSecondDuration(curTime, time);
        if (seconds > 0) {
            long m = seconds / 60;
            if (m < 10) {
                tvMinteShi.setText("0");
                tvMinuteGe.setText("" + m);
            } else {
                tvMinteShi.setText("" + (m / 10));
                tvMinuteGe.setText("" + (m % 10));
            }
            long s = seconds % 60;
            if (s < 10) {
                tvSecondShi.setText("0");
                tvSecondGe.setText("" + s);
            } else {
                tvSecondShi.setText("" + (s / 10));
                tvSecondGe.setText("" + (s % 10));
            }
            handler.postDelayed(runnable, 1000);
        } else {
            handler.removeCallbacks(runnable);
            ToastUtils.showCustomBgToast("订单超时");
            closeAct();
        }
    }
  
    @Override
    public void initEvent() {
        titleBar.setOnTitleBarClickListener(new ZzTitleBar.OnTitleBarClickListener() {
            @Override
            public void onLeftClick() {
                closeAct();
            }
  
            @Override
            public void onRightClick() {
  
            }
        });
  
        tvPay.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (cbAlipay.isChecked()) {
                    payV2();
                } else {
                    payVX();
                }
            }
        });
  
        llAlipay.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                cbAlipay.setChecked(true);
                cbWxpay.setChecked(false);
            }
        });
  
        llWxPay.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                cbWxpay.setChecked(true);
                cbAlipay.setChecked(false);
            }
        });
    }
  
    private void payVX() {
  
        Api.getRegisterApi(HCApplication.getContext())
                .vxPay(HCSharedUtil.getPhoneNumber(this), orderId)
                .compose(RxHelper.<VxPayResult>io_main())
                .subscribe(new Subscriber<VxPayResult>() {
                    @Override
                    public void onCompleted() {
  
                    }
  
                    @Override
                    public void onError(Throwable e) {
                        ToastUtils.showCustomBgToast(getString(R.string.no_net_text) + e.toString());
                    }
  
                    @Override
                    public void onNext(VxPayResult vxPayResult) {
                        if (vxPayResult.getCode().equals("1")) {
                            PayReq req = new PayReq();
                            req.appId = vxPayResult.getData().getAppid();
                            req.partnerId = vxPayResult.getData().getMch_id();
                            req.prepayId = vxPayResult.getData().getPrepay_id();
                            req.nonceStr = vxPayResult.getData().getOld_nonce_str();
                            req.packageValue = "Sign=WXPay";
                            req.sign = vxPayResult.getData().getOld_sign();
                            req.timeStamp = vxPayResult.getData().getTimestamp();
                            api.sendReq(req);
                            closeAct();
                        } else {
                            ToastUtils.showCustomBgToast(vxPayResult.getMsg());
                        }
                    }
                });
    }
  
    /**
     * 支付宝支付业务
     */
    public void payV2() {
  
        Api.getRegisterApi(HCApplication.getContext())
                .aliPay(HCSharedUtil.getPhoneNumber(this), orderId)
                .compose(RxHelper.<AliPayResult>io_main())
                .subscribe(new Subscriber<AliPayResult>() {
                    @Override
                    public void onCompleted() {
  
                    }

                    @Override
                    public void onError(Throwable e) {
                        ToastUtils.showCustomBgToast(getString(R.string.no_net_text) + e.toString());
                    }
  
                    @Override
                    public void onNext(AliPayResult aliPayResult) {
                        if (aliPayResult.getCode().equals("1")) {
                            final String orderInfo = aliPayResult.getOrderInfo();
  
                            Runnable payRunnable = new Runnable() {
                                @Override
                                public void run() {
                                    PayTask alipay = new PayTask(PayActivity.this);
                                    Map<String, String> result = alipay.payV2(orderInfo, true);
  
                                    Message msg = new Message();
                                    msg.what = SDK_PAY_FLAG;
                                    msg.obj = result;
                                    mHandler.sendMessage(msg);
                                }
                            };
  
                            Thread payThread = new Thread(payRunnable);
                            payThread.start();
                        }
                    }
                });
  
    }
  
    @Override
    public void saveState(Bundle bundle) {
  
    }
  
    @Override
    public void restoreState(Bundle bundle) {
  
    }
  
    @Override
    public boolean isDefaultBackClose() {
        return false;
    } 
  
    @Override
    protected void onDestroy() {
        super.onDestroy();
        handler.removeCallbacks(runnable);
        handler.removeCallbacksAndMessages(null);
        handler = null;
    }
}
```

## 注意点

- 支付宝支付后台需要*支付宝公钥*和*app私钥*。
- 支付宝支付App公私钥加密格式：Java和非Java。
- 微信商户平台需要设置API密钥。
- 微信支付后台需要二次签名。