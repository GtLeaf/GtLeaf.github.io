---
title: android多媒体——Vitamio
date: 2018-10-14 00:03:07
tags: android Vitamio
---
# 简介
	最近偶然抓到某直播平台的api，而且这也是Android的基础技能，所以突发奇想做个直播功能。
	用的库是Vitamio，虽然也有一点点小坑,但这个库用法简单，功能强大。
	能播放MKV,FLV,MP4等主流视频格式，也支持MMS, RTSP, RTMP, HLS(m3u8)等常见的多种视频流媒体协议。

# 1.导入与配置
参考博客：https://blog.csdn.net/qq_29078329/article/details/75039690
由于官网提供的是module，不是jar，所以要先从github下载：[Vitamio的github地址](https://github.com/yixia/VitamioBundle)

下好后android studio直接导入即可，建议新手将vitamio和vitamio-sample都导入
之后参考博客内容，在AndroidManifest中，注册io.vov.vitamio.activity.InitActivity：
```
<activity
            android:name="io.vov.vitamio.activity.InitActivity"
            android:configChanges="orientation|screenSize|smallestScreenSize|keyboard|keyboardHidden|navigation"
            android:launchMode="singleTop"
            android:theme="@android:style/Theme.NoTitleBar"
            android:windowSoftInputMode="stateAlwaysHidden" />
```
添加权限：
```
<uses-permission android:name="android.permission.WAKE_LOCK" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

# 2. 使用vitamio的VideoView
这一步我建议直接参考vitamio-samlp中的VideoViewBuffer.class，播放视频最基本的内容都包含在内：
![image.png](https://upload-images.jianshu.io/upload_images/7037957-9fb531cfef60d637.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
实在不行就将代码复制过去，先实现播放功能，在根据自己的需求修改。

# 问题
## 1.找不到 libvinit.so 文件
参考博客：https://www.jianshu.com/p/d08064bf6b85
具体错误内容：
Java.lang.UnsatisfiedLinkError: dalvik.system.PathClassLoader
[DexPathList[[zip file "/data/app/com.pckgname.live-2/base.apk"],
nativeLibraryDirectories=[/data/app/com.pckgname.live-2/lib/arm64, /vendor/lib64, /system/lib64]]] couldn't find "libvinit.so"

我们注意两处，UnsatisfiedLinkError和couldn't find "libvinit.so"。
是因为找不到libvinit.so，但这个文件确实在vitamio的libs文件夹下面。

![.so文件所在位置](https://upload-images.jianshu.io/upload_images/7037957-4ec9ef2ec4b1a6b2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
所以我们要给它指名位置，在app的gradle下加入:
```
ndk {
        abiFilters "armeabi", "armeabi-v7a", "x86", "mips"
    }
sourceSets {
    main {
        jniLibs.srcDirs = ['libs']
    }
}
```
![修改gradle](https://upload-images.jianshu.io/upload_images/7037957-8ebbac30e1e6b3a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 2.无法在暂时无法在Android api 23以上的版本运行
参考博客：https://blog.csdn.net/kuixiaoba/article/details/51490008
解决：同时把app和vitamio中的gradle，将目标SDK版本(targetSdkVersion)调低于23就可以了，如改成21.
![修改targetSdkVersion](https://upload-images.jianshu.io/upload_images/7037957-6cbced2cb8827cfd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
