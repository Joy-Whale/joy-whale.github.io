---
title: Android Studio 2.3
date: 2017-03-06 09:59:09
categories:
- 技术
- Android Blog
tags: 
- Android
- Android Studio
---
## 正文
现在Android Studio 2.3已经可以[下载](https://developer.android.com/studio/index.html)了。这个版本的亮点是对IDE质量的改进。

Android Studio 2.3有一些小的新功能设置贯穿整个开发流程。当设计你的应用程序时，在[布局编辑器](https://developer.android.com/studio/write/layout-editor.html)中更新了[WebP support](https://developer.android.com/studio/write/convert-webp.html)和[ConstraintLayout](https://developer.android.com/training/constraint-layout/index.html)支持库来更好地布局。当开发时，Android Studio 有一个新的[App Link Assistant](https://developer.android.com/studio/write/app-link-indexing.html)帮助你构建统一视图URLs。当构建和部署应用程序时，[Instant Run](https://developer.android.com/studio/run/index.html#instant-run)已经更直观和可靠。最后，当使用Android模拟器来测试应用时，现在有了好的副本支持。

<iframe width="560" height="315" src="https://www.youtube.com/embed/VFyKclKBGf0" frameborder="0" allowfullscreen></iframe>

请看看下面列表中关于改进后Android Studio 2.3新特性的更多细节：

### Build
- **Instant Run**的提升和图标的改变
作为质量提升上的一大亮点，Android Studio 2.3中的**Instant Run**已经变得更加可靠。**Run**按钮允许应用重新启动来反映代码的变化，新的**Apply Changes**按钮允许在程序保持运行的同时来尝试替换代码。我们通过底层实现来提高可靠性，我们还消除了**Instant Run**启用应用程序的启动延迟。[点击了解更多](https://developer.android.com/studio/run/index.html#instant-run)。

![image](http://ol58plgkm.bkt.clouddn.com/Screen%20Shot%202017-03-02%20at%209.12.10%20AM.png "新的Instant Run按钮")

- Build Cache
**Build Cache**已经介绍过在Android Studio 2.2默认禁用，**Build Cache**是Android Studio中一个底层构造用来优化项目构建。通过缓存AARs和pre-dexed外部库，新的build cached可以使**Build Clean**功能更加快速。这是一个user-wide缓存，在Android Studio 2.3中默认开启。[点击了解更多](http://d.android.com/studio/build/build-cache.html)。

### Design
- **Constraint Layout**对链(Chains)和比例(Ratios)的支持
Android Studio 2.3中包含了[ConstraintLayout](https://developer.android.com/reference/android/support/constraint/ConstraintLayout.html)的稳定版本。现在可以在一个ViewGroup中的同一方向上双向链接两个或更多的View。[点击了解更多](https://developer.android.com/training/constraint-layout/index.html#constrain-chain)。

![image](http://ol58plgkm.bkt.clouddn.com/chains_support.gif "Constraint Layout的Chains")

Constraint Layout也支持比例，可以帮助你在保持View的长宽比的情况下扩大或缩小View。[点击了解更多](http://d.android.com/training/constraint-layout/index.html#ratio)。此外，** ConstraintLayout**的Chains和Ratios可以通过[ConstraintSet APIs](https://developer.android.com/reference/android/support/constraint/ConstraintSet.html)支持编程创建。

![image](http://ol58plgkm.bkt.clouddn.com/ratio_support.gif "Constraint Layout的Ratios")

- Layout Editor Palette
更新后的Layout Editor添加了搜索、排序和过滤功能，在拖到设计界面前可以看到部件预览图。
[点击了解更多](https://developer.android.com/studio/write/layout-editor.html)。

![image](http://ol58plgkm.bkt.clouddn.com/Screen Shot 2017-03-02 at 9.14.58 AM.png "Layout Editor Widget Palette")

- Layout Favorites
更新后的Layout Editor可以保存你常用的小部件的属性。当在高级选项中给属性添加一颗星后，它将会在Favorite选项中出现。

![image](http://ol58plgkm.bkt.clouddn.com/fav_attributes.gif "Favorites Attributes on Layout Editor Properties Panel")

- WebP Support
**WebP Support**可以帮助我们减小Apk的体积，Android Studio现在可以把项目中的PNG图片生成WebP图片。WebP的无损格式比一个PNG图片小约25%。使用Android Studio 2.3，我们可以转换PNG为无损WebP，也可以检查有损WebP编码。右击non-launcher的PNG图片去转换为WebP。如果需要编辑图片，我们也可以右击一个WebP文件去转换回PNG图片。[点击了解更多](https://developer.android.com/studio/write/convert-webp.html)。

![image](http://ol58plgkm.bkt.clouddn.com/Screen Shot 2017-03-02 at 9.17.47 AM.png "WebP Image Conversion Wizard")

- Material Icon Wizard Update
更新后的Vector Asset Wizard支持搜索和过滤，并且为每个Icon都添加了标签。[点击了解更多](https://developer.android.com/studio/write/vector-asset-studio.html#materialicon)。

![image](http://ol58plgkm.bkt.clouddn.com/Screen Shot 2017-03-02 at 9.18.30 AM.png "Vector Asset Wizard")

### Develop
- Lint Baseline
Android Studio 2.3中可以在项目中设置自定义的未解决行警告提示。从此以后，Lint只会提示新的问题。如果项目中有很多遗留的lint问题时是非常有用的，但只是想关注解决新问题。[点击了解更多](https://developer.android.com/studio/write/lint.html#snapshot)关于Lint baseline和[new Lint checks & annotations](https://developer.android.com/studio/releases/index.html)。

![image](http://ol58plgkm.bkt.clouddn.com/Screen Shot 2017-03-02 at 9.19.09 AM.png "Lint Baseline Support")

- App Links Assistant
Android Studio现在支持Android App Links将更加简单。新的App Links Assistant可以使我们更简单地去创建一个新的URLs的intent filters。通过一个Digital Asset Links文件去申明app网址联想，然后测试Android App Links。点击**Tools → App Link Assistant**去使用 App Link Assistant。[点击了解更多](https://developer.android.com/studio/write/app-link-indexing.html)。

![image](http://ol58plgkm.bkt.clouddn.com/Screen Shot 2017-03-02 at 9.19.52 AM.png "App Links Assistant")

- Template Updates
默认情况下，Android Studio 2.3中所有模板使用*RelativeLayout*，现在使用*ConstraintLayout*。点击了解更多关于[templates](https://developer.android.com/studio/projects/templates.html)和[ConstraintLayout](https://developer.android.com/training/constraint-layout/index.html)。

![image](http://ol58plgkm.bkt.clouddn.com/Screen Shot 2017-03-02 at 9.20.57 AM.png "New Project Wizard Templates")

- IntelliJ Platform Update
Android Studio 2.3包含了IntelliJ 2016.2，它增强了更新检查窗口和通知系统等。[点击了解更多](https://www.jetbrains.com/idea/whatsnew/#v2016-2)。

### Test
- Android模拟器副本
在最新的模拟器版本(v25.3.1)中添加了模拟器副本功能，我们有一个Android模拟器之间的共享剪贴板和主机操作系统，它允许我们在不同的环境中复制文字。该功能支持API Level 19或以上的x86 Google API模拟器系统。

![image](http://ol58plgkm.bkt.clouddn.com/emulator_copy_paste.gif "Copy & Paste support in Android Emulator")

- Android模拟器命令行工具
从Android Sdk Tools 25.3开始，我们把emulator文件夹从SDK Tools文件夹中移动到一个单独的emulator文件夹中，并且使用单独的[avdmanager命令行](https://developer.android.com/studio/command-line/avdmanager.html)取代了android avd命令行，更新后即可使用。我们也为模拟器命令行添加了位置重定向功能。然而，使用命令行创建的Android Virtual Devices (AVDs)需要更新相应的脚本。使用Android Studio 2.3来创建模拟器不会受到影响。[点击了解更多](https://developer.android.com/studio/releases/sdk-tools.html)。

### Getting Started

#### Download
若你现在使用的是旧的版本，可以在导航栏菜单中检测和更新稳定版(Help → Check for Update [Windows/Linux] , Android Studio → Check for Updates [OS X])，也可以在[下载页面](https://developer.android.com/studio/index.html)中下载。当然也需要将项目中的Android Gradle plugin版本更新到2.3.0来使用Android Studio2.3的这些新特性。

### [原文链接](https://android-developers.googleblog.com/2017/03/android-studio-2-3.html)
