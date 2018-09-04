---
title: 使用 Zxing 库简单实现二维码生成与扫描
date: 2016-12-17 14:34:27
tags: ["zxing"]
categories: ["zxing"]
---

#### 目录

>  
1、示例项目结构介绍  
2、复制文件  
3、码好代码  
4、做好配置  
5、查看效果  

示例代码放在 Github 上：[ZxingSmall](https://github.com/xiaosongfu/ZxingSmall)  

### 1、示例项目结构介绍  

![你看不到我^_^](https://lollipop.xiaosongfu.com/blog/201612/zxing01.jpg)  

如图，MainActivity 是主界面，有两个按钮，一个启动扫描二维码界面：CaptureActivity ，一个启动生成二维码界面：GenerateQRCodeActivity 。  

### 2、复制文件

>  
1. 复制 zxing 包下的所有 java 文件  
2. 复制 raw 目录下的 beep.ogg ，这是扫描成功后的提示音  
3. 复制 values 目录下的 attrs.xml 、ids.xml 文件  
4. 复制 values 目录下的 colors.xml 、strings.xml 中与 zxing 相关的属性  
5. 通过启动 CaptureActivity 来启动二维码扫描界面，所以需要复制 layout 目录下的 activity_capture.xml ，它里面 include 了 scanner_toolbar.xml ，所以也需要复制它， scanner_toolbar.xml 又使用到了 abc_ic_back_mtrl_am_alpha.png 图片，所以也需要复制它  

### 3、码好代码

##### 3.1 扫描二维码

使用 startActivityForResult(...) 方法启动 CaptureActivity ，并在 onActivityResult(...) 方法里接收扫描结果：  
```  
    // REQUEST_CODE
    private static final int REQUEST_CODE = 100;

    /**
     * 启动  ，开始二维码的扫描
     *
     * @param view view
     */
    public void scanQRCode(View view){
        Intent intent = new Intent(this, CaptureActivity.class);
        startActivityForResult(intent, REQUEST_CODE);
    }

    /**
     * onActivityResult 用于接收二维码扫描结果
     *
     * @param requestCode requestCode
     * @param resultCode resultCode
     * @param data data
     */
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (resultCode == RESULT_OK) {
            Bundle bundle = data.getExtras();
            String scanResult = bundle.getString("result");
            Toast.makeText(this, scanResult, Toast.LENGTH_LONG).show();
        }
    }
```  

##### 3.2 生成二维码

代码十分简单：

```  
try {
        Bitmap mBitmap = EncodingHandler.createQRCode("www.baidu.com", 300);
        ((ImageView)findViewById(R.id.image)).setImageBitmap(mBitmap);
    } catch (Exception e) {
        e.printStackTrace();
    }
```  

### 4、做好配置

##### 4.1 权限配置

```  
<!-- 震动权限 -->
<uses-permission android:name="android.permission.VIBRATE" />
<!-- 相机权限 -->
<uses-permission android:name="android.permission.CAMERA" />
<!-- 闪光灯权限 -->
<uses-permission android:name="android.permission.FLASHLIGHT"/>
<!-- 使用照相机权限 -->
<uses-feature android:name="android.hardware.camera" />
<!-- 自动聚焦权限 -->
<uses-feature android:name="android.hardware.camera.autofocus" />
```  

##### 4.2 Activity 配置：
```   
<activity android:name=".zxing.activity.CaptureActivity"/>
```  

### 5、查看效果

![你看不到我^_^](https://lollipop.xiaosongfu.com/blog/201612/zxing02.png)  