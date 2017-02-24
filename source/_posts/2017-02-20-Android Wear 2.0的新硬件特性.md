layout: android
title: Android Wear 2.0的新硬件特性
date: 2017-02-20 11:55:17
categories:
- 技术
- Android Blog
tags: 
- Android
- Android Developers Blog
- Android Wear 2.0
---
![image](http://ol58plgkm.bkt.clouddn.com/Screen%20Shot%202017-02-09%20at%209.28.25%20AM.png)

## 正文
今天我们发布了Android Wear 2.0最终版，在这次发布中，我们添加了对[new hardware features announced yesterday](https://blog.google/products/android-wear/android-wear-20-make-most-every-minute/)的支持。如果你没有这样做，真的是时候[发布您的应用程序](https://developer.android.com/wear/preview/features/app-distribution.html)，以便不错过明天以后的用户的使用。

在开发者预览版程序，你给了我们很多建设性的反馈以及bug报告。再一次感谢您!

### Android Wear 2.0回顾

<iframe width="560" height="315" src="https://www.youtube.com/embed/-F-llkD6cQY" frameborder="0" allowfullscreen></iframe>

Android Wear 2.0是我们从2014年以来最大的更新，包含许多平台和开发人员改进。
包含一些亮点：
- [Material Design for Android Wear](https://www.google.com/design/spec-wear/)一个新系统的用户界面和设计指南，一个暗色模式的特性，垂直布局和视觉组件，例如WearableRecyclerView和WearableNavigationDrawer。我们也通过新MessagingStyle丰富的通知样式和联机活动增强了watch通知。
- [Watch Face Complications](https://developer.android.com/training/wearables/watch-faces/complications.html) Complications是在watch faces上显示信息以外的时间的区域，应用程序可以通过创建一个ComplicationProviderService来提供数据支持，watch faces可以以watch faces样式渲染这些数据。
- [Standalone Android Wear apps and iOS support](https://developer.android.com/training/wearables/apps/standalone-apps.html)我们可以直接通过on-watch Google Play Service下载应用到Wear设备。此外,这些应用程序可以不用依赖手机应用程序直接访问网络。这意味着应用程序可以在Android Wear设备上运行并配对iphone设备。

### 新的硬件支持
前两个Android Wear 2.0 watches使用户有更多的方式去体验他们的智能手表。在最终版SDK中，我们添加了对物理[按钮位置](https://developer.android.com/training/wearables/ui/multi-function.html)和[旋转输入](https://developer.android.com/training/wearables/ui/rotary-input.html)的API的支持。目前,开发人员将需要新的LG Watch Style或LG Watch Sport测试这些新功能；然而，我们正在努力把这些新的硬件特性添加到模拟器中。请继续关注更新！SDK还包括其他一些最后的bug修复，比如在Wearable Action Drawer中支持超过三个选项卡。

### 应用审查变化
现在，Android Wear 2.0已经上线，不久后我们将会更新两个重要的[Android Wear App Quality](https://developer.android.com/distribute/essentials/quality/wear.html)审查流程。首先，用你的手机app来增强Android Wear将不予通过。其次，你需要尽快上传一个适配Android Wear 2.0的watch应用。应用符合这些标准才能上架到手机Play Store并且有资格进入Android Wear apps热门排行榜。这些变化将确保一个更一致的用户体验和允许我们简化评审过程。

### 这并不是终点
Android Wear 2.0开发预览版比我们原有的计划持续的时间更长，但我们认为额外的时间在很大程度上得到了回报。再次感谢您的反馈和耐心。有你们的帮助我们会做得更好。

我们已经将[https://developer.android.com/wear/index.html](https://developer.android.com/wear/index.html)集成到Android Wear2.0的开发者预览版文档中，并且我们将继续保持[ factory images for the preview devices]( factory images for the preview devices)的链接。请提交bug到[file developer bug reports](https://code.google.com/p/android/issues/entry?template=Android%20Wear%20bug%20report)或是在Android Wear Developers](https://plus.google.com/communities/113381227473021565406)内发表评论。

来自Android Wear团队:再次感谢您的反馈和支持!

### [原文链接](https://android-developers.googleblog.com/2017/02/AndroidWear2.html)