---
title: 最新版本Support Library 25.2.0
date: 2017-02-27 18:13:35
categories:
- 技术
- Android Blog
tags: 
- Android Support Library
- Android FlexboxLayout
---
## 正文
持续更新技术是很重要的。这也是我们为什么一直在努力提高我们的软件质量，特别是链接到您的应用程序的library，例如[Support Library](https://developer.android.com/topic/libraries/support-library)。*Support Library*是一个许多Android版本中提供向后兼容性以及附加功能的库。

我们刚刚发布了[Support Library 25.2.0](https://developer.android.com/topic/libraries/support-library/revisions.html)。如果你在25.1.1或 25.1.0中使用**android.support.v7.media.MediaRouter**类，由于已知的问题我们强烈建议您更新。如果你最近没有更新，你已经错过了一些大的bug修复，如这些：

25.2:
- 修正在使用A2DP蓝牙设备和媒体路由的API时可能导致的设备mediarouter变得严重反应迟钝的问题，需要重新启动。
- 使用屏幕镜像显示幻灯片不再导致设备与Wi-Fi断开连接。
- 当没有使用**setMediaButtonReceiver()**绑定时，媒体按钮现在可以妥善处理媒体应用程序
- *TextInputLayout*在文字由XML设置时可以正确地覆盖提示和文字 (AOSP issue 230171)。
- 修正*MediaControllerCompat*中的内存泄漏 (AOSP issue 231441)。
- *RecyclerView*在循环回收View holders时不再崩溃 (AOSP issue 225762)。

### 报告和修复bug
*Support Library*是由*Android Framework*和类似于Android平台的*Developer Relations*团队共同开发，你可以使用[AOSP issue tracker](https://code.google.com/p/android/issues/list)来提交bug，或是提交到我们的[Github](https://android.googlesource.com/platform/frameworks/support.git)仓库。

### [原文链接](https://android-developers.googleblog.com/2017/02/keeping-up-to-date-with-the-support_23.html)
