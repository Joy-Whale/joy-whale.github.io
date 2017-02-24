layout: android
title: Android Wear 2.0开发预览终极版发布：适配ios。是时候上传你的apps到Play Store
date: 2017-02-17 17:05:53
categories:
- 技术
- Android Blog
tags: 
- Android
- Android Developers Blog
- Android Wear 2.0
---
## 正文
![image](http://ol58plgkm.bkt.clouddn.com/Google-Android---Blog-Post-iPhone_1200x675px_v3.gif)
目前，我们发布了第五个也是最后一个开发预览版Android Wear 2.0。在这次发布中，我们添加了对ios的支持，包括大量的bug修复和增强。通过这个预览版本编译的app现在已经可以提交到Google Play Store，所以，是时候[上传你的apps](https://developer.android.com/wear/preview/features/app-distribution.html)了。Android Wear 2.0在二月初发布，我们感谢大家在开发者预览版本时期的积极反馈。您的意见帮助我们发现bugs以及驱动关键产品决策，再次感谢。
###iOS Support
从[2015](https://googleblog.blogspot.com/2015/08/android-wear-now-works-with-iphones.html)年以来，用户就可以用iphones与Android Wear配对，现在可以更好地使用iphones来配对wears。开发者只需要在app manifest文件中设置[standalone=true](https://developer.android.com/wear/preview/features/app-distribution.html#specifying-app-as-standalone)标记即可使用该功能。它可以使Play Store知道你的watch app不需要一个安卓手机应用，因此可以出现在iphone的Play Store手表搭配。开发者可以参考[这些步骤](https://developer.android.com/wear/preview/support.html#iphone-companion)来配对一个iphone手机并测试。
```
<application …>
   <meta-data android:name="com.google.android.wearable.standalone" android:value="true"/>
    …
</application>
```
独立的应用程序可用的网络带宽可以低于预期，平台平衡电池储蓄和网络带宽。一定要看看这些[指导方针](https://developer.android.com/wear/preview/features/standalone-apps.html#high-bandwidth-network-access)来访问网络，手表搭配iphone可以访问WIFI和蜂窝网络。

用这个开发预览版，Android Wear apps运行在手表上并配对iOS设备将能够执行手机上的一些操作，如[OAuth](https://developer.android.com/wear/preview/features/auth-wear.html#OAuth)和[RemoteIntent](https://developer.android.com/wear/preview/features/standalone-apps.html)为iOS设备上启动一个web页面。

### 上传你的应用到Google Play Store
这个开发预览版提供一个可穿戴Support Library的更新，apps使用api 25编译已经可以在Google Play Store上部署了。请注意，该开发预览版在watches或模拟器上并没有更新预览。

### 其他改进和错误修正
- Navigation Drawer: [轻击一个标签](https://developer.android.com/wear/preview/features/ui-nav-actions.html#create-a-drawer)切换到单页上，icon-only导航抽屉，提供更快、更合理的页面跳转导航。
- NFC HCE support: [NFC Host Card Emulation](https://developer.android.com/guide/topics/connectivity/nfc/hce.html) FEATURE_NFC_HOST_CARD_EMULATION 现在已经支持。
- ProGuard and Complication API: 新的混淆配置意味着并发症数据容器类将不再是混淆视听，它修复了当watch正试图访问一个复杂的数据引发的ClassNotFoundException。

### Countdown to Launch

谢谢开发人员的重要反馈，检出[g.co/wearpreview](https://g.co/wearpreview)最新版本以及文档，并确保在2月初Android Wear 2.0消费者版本推出前发布您的应用程序。当我们推出该版本后,请继续[提交bug](https://g.co/wearpreviewbug)或在[Android Wear Developers社区](https://plus.google.com/communities/113381227473021565406)发表评论。我们等不及要看你的Android Wear2.0的应用程序！

### [原文链接](https://android-developers.googleblog.com/2017/01/final-android-wear-20-developer-preview.html)