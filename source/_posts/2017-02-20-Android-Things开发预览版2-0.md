---
title: Android Things开发预览版2.0
date: 2017-02-20 10:49:50
categories:
- 技术
- Android Blog
tags: 
- Andorid
- Android Developers Blog
- Android Things
---
## 正文
今天我们发布了Android Things 2.0，修复了一些bug以及添加了一些新的功能。我们致力于为开发人员提供定期更新，大约每6 - 8周推出新的预览版本。Android Things是一个构建Android物联网(物联网)产品的全面的解决方案。现在任何Android开发者可以使用谷歌Android api和服务快速构建一个智能设备，并且可以从谷歌保持高度安全更新。它包括开发者熟悉的开发工具，Android Studio，安卓软件开发工具包(SDK)，Google Play Service，Google云平台。 Android Things支持System-on-Module(SoM)体系结构，在最初的核心计算模块可以使用开发板，然后轻松地扩展到大型生产运行与定制设计，同时从谷歌继续使用相同的 Board Support Package(BSP)。

### 新的功能以及Bug修复
感谢开发者对开发预览版1的反馈，我们已经添加了对Intel Edison和Raspberry Pi 3的USB音频硬件抽象层(HAL)的支持。NXP Pico已经包含直接对音频设备的支持。我们也解决了很多Peripheral I/O(PIO)相关的bug。其他例如支持蓝牙等已知的新功能需求，团队正在积极努力解决这些问题。我们已经添加了对最具运算能力的Intel Joule平台的支持。

### Native I/O和用户驱动程序
很多开发者使用native c/c++去开发IoT程序，Android Things支持标准的Android NDK。我们已经发布了一个API库提供本机访问[Peripheral API](https://developer.android.com/things/sdk/pio/index.html)(PIO)，所以开发者可以很轻松地使用他们现有的native代码。[Documentation](https://developer.android.com/things/sdk/pio/native.html)有新API的说明和[使用实例](https://github.com/androidthings/sample-nativepio)演示如何使用它们。

一个重要的新特性，可用Android ThingsDP1支持[用户驱动程序](https://developer.android.com/things/sdk/drivers/index.html)，开发人员可以创建一个用户驱动的APK，然后将其绑定到Framework。例如，你的驱动程序可以读取GPIO pin或是触发一个Android KeyEvent，或通过串口读取外部GPS和反馈到Android location APIs。这允许任何没有定制Linux内核或HAL的应用程序注入硬件事件框架。我们维护的存储库用户驱动传感器等各种常见的硬件接口,按钮,和显示。开发人员也能够创建自己的设备然后在社区分享。

### TensorFlow for Android Things
Android Things一个十分有趣的功能是容易部署的机器学习和计算机视觉的能力。我们已经创建了一个请求[示例](https://github.com/androidthings/sample-tensorflow-imageclassifier)，展示了如何使用[TensorFlow](https://www.tensorflow.org/) Android Things设备。这个示例展示了访问摄像头，进行目标识别和图像分类，并使用语音(TTS)公开结果。一个预建ARM和X86的抢先体验TensorFlow推理库提供给开发者一个简单的方式去在任何Android应用中添加TensorFlow，而这仅仅需要在你的build.gradle文件中添加一行代码。

![image](http://ol58plgkm.bkt.clouddn.com/tensorflow_sample_dog.png)

### 反馈
感觉之前为开发预览版提供反馈的开发者们，请继续反馈给我们你的[bug报告](https://code.google.com/p/android/issues/entry?template=Android%20Things%20bug%20report)和[新功能需求](https://code.google.com/p/android/issues/entry?template=Android%20Things%20feature%20request)，也可以在[stackoverflow](http://stackoverflow.com/questions/tagged/android-things)提问。要下载Developer Preview 2镜像，请访问Android Things[下载页](https://developer.android.com/things/preview/download.html)，也可以在[release notes](https://developer.android.com/things/preview/releases.html)中查看新版本特性。你也可以在Google+中加入[Google's IoT Developers Community](https://g.co/iotdev)，这里有不断更新的资源也可以讨论想法，有大概2900个新成员。

![image](http://ol58plgkm.bkt.clouddn.com/Screen%20Shot%202017-02-08%20at%2010.21.46%20PM.png)

### [原文链接](https://android-developers.googleblog.com/2017/02/android-things-developer-preview-2.html)