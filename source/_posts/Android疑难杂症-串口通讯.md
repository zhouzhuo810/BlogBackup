---
title: Android疑难杂症-串口通讯
date: 2017-08-23 11:04:21
tags:
	- Android
categories: Android疑难杂症
---

最近公司突然要求android客户端与串口通信。

我的第一想法是查看Android Api文档。

但是官方文档只是介绍了如何添加权限；如何监听usb设备；

对于设置通信时的波特率等参数并没有详细介绍。

我只能从第三方入手了。

找了两天，终于有了成果。

[usb-serial-for-android](https://github.com/mik3y/usb-serial-for-android)

那就是这个项目。

用法简单。功能强大。

下面贴出我的串口读写代码，仅供参考。以后有空再整理下。

<!-- more -->

```xml
//manifest.xml
    <uses-feature android:name="android.hardware.usb.host"/>
	<activity
	    android:name="..."
	    ...>
	  <intent-filter>
	    <action android:name="android.hardware.usb.action.USB_DEVICE_ATTACHED" />
	  </intent-filter>
	  <meta-data
	      android:name="android.hardware.usb.action.USB_DEVICE_ATTACHED"
	      android:resource="@xml/device_filter" />
	</activity>
```

```xml
<!-- 和你的usb转串口的芯片的型号有关 -->
<!--xml/usbfilter.xml-->
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <!--<usb-device vendor-id="1234" product-id="5678" />-->
    <usb-device vendor-id="0403" product-id="6001" />
</resources>
```

```java
    private UsbManager manager;
  
    private UsbSerialDriver driver;
    private UsbSerialPort port;
  
  	//读数据线程
    private Runnable readThread;
  
//onCreate中
  
        manager = (UsbManager) getSystemService(Context.USB_SERVICE);
  
        readThread = new Runnable() {
            @Override
            public void run() {
                while (port != null && !Thread.interrupted()) {
                    final byte[] data = new byte[100];
                    try {
                        int num = port.read(data, 3000);
                        if (num > 0) {
                            runOnUiThread(new Runnable() {
                                @Override
                                public void run() {
                                    generateData(data);
                                }
                            });
                        }
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                    //0.5秒读一次。
                    try {
                        Thread.sleep(500);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        };
```

```java
private void openUsbDevice() {
    //before open usb device
    //should try to get usb permission
    tryGetUsbPermission();
}
  
private static final String ACTION_USB_PERMISSION = "com.android.example.USB_PERMISSION";
  
private void tryGetUsbPermission() {
  
    PendingIntent mPermissionIntent = PendingIntent.getBroadcast(this, 0, new Intent(ACTION_USB_PERMISSION), 0);
  
    //here do emulation to ask all connected usb device for permission
    for (final UsbDevice usbDevice : manager.getDeviceList().values()) {
        //add some conditional check if necessary
        //if(isWeCaredUsbDevice(usbDevice)){
        if (manager.hasPermission(usbDevice)) {
            //if has already got permission, just goto connect it
            //that means: user has choose yes for your previously popup window asking for grant perssion for this usb device
            //and also choose option: not ask again
            afterGetUsbPermission(usbDevice);
        } else {
            //this line will let android popup window, ask user whether to allow this app to have permission to operate this usb device
            manager.requestPermission(usbDevice, mPermissionIntent);
        }
        //}
    }
}
  
  
private void afterGetUsbPermission(UsbDevice usbDevice) {
    //call method to set up device communication
    ToastUtil.showCustomBgToast(MyApplication.getContext(), String.valueOf("Got permission for usb device: " + usbDevice + String.valueOf("\nFound USB device: VID=" + usbDevice.getVendorId() + " PID=" + usbDevice.getProductId())));
    doYourOpenUsbDevice(usbDevice);
}
  
private void doYourOpenUsbDevice(UsbDevice usbDevice) {
  
    // Find all available drivers from attached devices.
    List<UsbSerialDriver> availableDrivers = UsbSerialProber.getDefaultProber().findAllDrivers(manager);
    if (availableDrivers.isEmpty()) {
        ToastUtil.showCustomBgToast(MyApplication.getContext(), "no available device");
        return;
    }
  
    // Open a connection to the first available driver.
    UsbSerialDriver driver = availableDrivers.get(0);
    UsbDeviceConnection connection = manager.openDevice(driver.getDevice());
    if (connection == null) {
        return;
    }
    // Read some data! Most have just one port (port 0).
    port = driver.getPorts().get(0);
    try {
        port.open(connection);
        port.setParameters(115200, 8, UsbSerialPort.STOPBITS_1, UsbSerialPort.PARITY_NONE);
    } catch (IOException e) {
        e.printStackTrace();
    }
    new Thread(readThread).start();
}
```

```java
private final BroadcastReceiver mUsbPermissionActionReceiver = new BroadcastReceiver() {
  
    public void onReceive(Context context, Intent intent) {
        String action = intent.getAction();
        if (UsbManager.ACTION_USB_DEVICE_ATTACHED.equals(action)) {
            // Find all available drivers from attached devices.
            List<UsbSerialDriver> availableDrivers = UsbSerialProber.getDefaultProber().findAllDrivers(manager);
            if (availableDrivers.isEmpty()) {
                ToastUtil.showCustomBgToast(MyApplication.getContext(), "no available device");
                return;
            }
            // Open a connection to the first available driver.
            driver = availableDrivers.get(0);
            try {
                UsbDeviceConnection connection = manager.openDevice(driver.getDevice());
                if (connection == null) {
                    openUsbDevice();
                    return;
                }
                port = driver.getPorts().get(0);
                try {
                    port.open(connection);
                    //波特率在这设置
                    port.setParameters(115200, 8, UsbSerialPort.STOPBITS_1, UsbSerialPort.PARITY_NONE);
                } catch (IOException e) {
                    e.printStackTrace();
                }
                new Thread(readThread).start();
            } catch (SecurityException e) {
                e.printStackTrace();
            }
        } else if (UsbManager.ACTION_USB_DEVICE_DETACHED.equals(action)) {
            if (port != null) {
                try {
                    port.close();
                    port = null;
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        } else if (ACTION_USB_PERMISSION.equals(action)) {
            synchronized (this) {
                UsbDevice usbDevice = (UsbDevice) intent.getParcelableExtra(UsbManager.EXTRA_DEVICE);
                if (intent.getBooleanExtra(UsbManager.EXTRA_PERMISSION_GRANTED, false)) {
                    //user choose YES for your previously popup window asking for grant perssion for this usb device
                    if (null != usbDevice) {
                        afterGetUsbPermission(usbDevice);
                    }
                } else {
                    //user choose NO for your previously popup window asking for grant perssion for this usb device
                    Toast.makeText(context, String.valueOf("Permission denied for device" + usbDevice), Toast.LENGTH_LONG).show();
                }
            }
        }
    }
};
```

```java
@Override
protected void onResume() {
    super.onResume();
    IntentFilter usbFilter = new IntentFilter();
    usbFilter.addAction(UsbManager.ACTION_USB_DEVICE_ATTACHED);
    usbFilter.addAction(UsbManager.ACTION_USB_DEVICE_DETACHED);
    usbFilter.addAction(ACTION_USB_PERMISSION);
    registerReceiver(mUsbPermissionActionReceiver, usbFilter);
}
  
@Override
protected void onPause() {
    super.onPause();
    unregisterReceiver(mUsbPermissionActionReceiver);
}
```

```java
@Override
protected void onDestroy() {
    super.onDestroy();
    if (port != null) {
        try {
            port.close();
            port = null;
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

```java
//点击读写按钮时操作。
  
        btnRead.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                try {
                	//读取命令，根据自己通讯协议修改
                    byte data[] = new byte[]{0x55, 0x33, 0x01, 62, (byte) 0xB1, 0x27};
                    if (port != null) {
                        int num = port.write(data, 500);
                    } else {
                        ToastUtil.showCustomBgToast(MyApplication.getContext(), "未检测到设备，请重新插入");
                    }
                } catch (IOException e) {
                    // Deal with error.
                    ToastUtil.showCustomBgToast(MyApplication.getContext(), e.getMessage());
                }
            }
        });
  
  
        btnWrite.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                try {
                    if (port != null) {
                        //数据修改
                        if (checkData()) {
                            //CRC校验，根据自己通讯协议修改。
                            byte[] data = CRC16.CRC_XModem(write_data);
                            int num = port.write(data, 1000);
                        }
                    } else {
                        ToastUtil.showCustomBgToast(MyApplication.getContext(), "未检测到设备，请重新插入");
                    }
                } catch (IOException e) {
                    e.printStackTrace();
                    ToastUtil.showCustomBgToast(MyApplication.getContext(), e.getMessage());
                }
//                ToastUtil.showCustomBgToast(MyApplication.getContext(), DataUtil.byteArrayToString(DataUtil.intToBytes(9999)));
            }
  
        });
```