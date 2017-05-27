---
title: 使用FlexboxLayout创建柔性layouts
date: 2017-02-22 16:44:38
categories:
- 技术
- Android Blog
tags: 
- Android
- Android FlexboxLayout
---
## 正文
去年Google I/O宣布了[ConstraintLayout](https://developer.android.com/training/constraint-layout/index.html)，能够在保持平面视图层次结构的同时构建复杂布局，并且可以完美地适配Android Studio的[Visual Layout Editor](https://developer.android.com/training/constraint-layout/index.html)。

同时，我们开源[FlexboxLayout](https://github.com/google/flexbox-layout)带来了Android与CSS布局灵活模块相同的功能。某些场景下*FlexboxLayout*将会非常有用。

_FlexboxLayout_由于和_LinearLayout_都可以布局调整它们子View的顺序，所以可以解释为一种先进的*LinearLayout*。不同点在于*FlexboxLayout*具有包装的特性。

这意味着如果添加**flexWrap="wrap"**属性，当子View没有足够的空间放在当前行时*FlexboxLayout*将会把它放在新的一行，如下图所示：

![image](http://ol58plgkm.bkt.clouddn.com/Untitled_document0.png)

### 各种屏幕尺寸下的layout
这种特性有什么用处？让我们举个栗子，假如你想把views按顺序排布，但由于屏幕线性可用空间发生变化时（由于设备因素、方向的变化或窗口大小的多窗口模式），你需要将后面的views换行排布。

![image](http://ol58plgkm.bkt.clouddn.com/Untitled_document1.png "Nexus5X竖屏")

![image](http://ol58plgkm.bkt.clouddn.com/Untitled_document2.png "Nexus5X横屏")

![image](http://ol58plgkm.bkt.clouddn.com/Untitled_document3.png "Pixel C多屏模式下，分割线在左侧")

![image](http://ol58plgkm.bkt.clouddn.com/Untitled_document4.png "Pixel C多屏模式下，分割线在中间")

![image](http://ol58plgkm.bkt.clouddn.com/Untitled_document5.png "Pixel C多屏模式下，分割线在右侧")

你可能需要定义多个DP-bucket布局（例如：layout-600dp, layout-720dp, layout-1020dp）来使用传统layouts（例如：*LinearLayout*或者*RelativeLayout*）去处理各种各样的屏幕尺寸。但是上面图片中的布局仅仅使用了一个*FlexboxLayout*。

上面栗子中使用的技术是设置**flexWrap="wrap"**。

```
<com.google.android.flexbox.Flexboxlayout 
     android:layout_width="match_parent" 
     android:layout_height="wrap_content" 
     app:flexwrap="wrap">
```

然后，您可以得到以下布局，其中子视图与新的线对齐，而不是溢出其父。

![image](http://ol58plgkm.bkt.clouddn.com/Untitled_document6.png)

另一种技术，我想降调的是在某个子View中设置*layout_flexgrow*属性。这有益于剩余有多余的空间时，改善最终的布局效果。*layout_flexgrow*属性类似*LinearLayout*中的*layout_weight*属性，这意味着*FlexboxLayout*会根据每个子View在同一条线上的*layout_flexgrow*值来分配剩余空间。

在下面的栗子中，假设每个子View的*layout_flexgrow*值都为1，那么多余的空间将会被均匀地分布到每个子View中。

```
 <android.support.design.widget.TextInputLayout
     android:layout_width="100dp"
     android:layout_height="wrap_content" 
     app:layout_flexgrow="1">
```

![image](http://ol58plgkm.bkt.clouddn.com/Untitled_document7.png)

你可以在Github库里检出完整的[layout.xml](https://github.com/google/flexbox-layout/blob/master/app/src/main/res/layout/fragment_flex_item_edit.xml)。

### 结合RecyclerView
*FlexboxLayout*的另一个优点是可以结合[*RecyclerView*](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)来使用。最新发布的[alpha version](https://github.com/google/flexbox-layout/blob/master/RecyclerView.md)中新的*FlexboxLayoutManager*继承自*RecyclerView.LayoutManager*，现在你可以用更高效的内存在可滚动的容器中使用flexbox的功能。

需要注意的是，你仍然可以使用*ScrollView*嵌套*FlexboxLayout*来实现可滚动的flexbox布局。但是当布局中的子item数量十分大时可能会出现流畅度问题甚至是*OutOfMemoryError*，当子View滚动离开屏幕时，*FlexboxLayout*不会回收它们。

（如果你想学习*RecyclerView*更多详细的知识，你可以从Android UI toolkit team检出相关视频，例如[1](https://www.youtube.com/watch?v=LqBlYJTfLP4),[2](https://www.youtube.com/watch?v=KhLVD6iiZQs)）

在*FlexboxLayout*库中有一个[栗子](https://github.com/google/flexbox-layout/tree/dev_recyclerview/demo-cat-gallery)，你可以看到，在RecyclerView中的每个图像的宽度都不同，但通过设置flexwrap，
```
FlexboxLayoutManager layoutManager = new FlexboxLayoutManager();
layoutManager.setFlexWrap(FlexWrap.WRAP);
```
和设置*flexGrow*（正如你所见，你可以通过*FlexboxLayoutManager*和*FlexboxLayoutManager.LayoutParams*来设置这些属性来代替在xml文件中配置它们）属性去调整它们。
```
void bindTo(Drawable drawable) {
  mImageView.setImageDrawable(drawable);
  ViewGroup.LayoutParams lp = mImageView.getLayoutParams();
  if (lp instanceof FlexboxLayoutManager.LayoutParams) {
    FlexboxLayoutManager.LayoutParams flexboxLp = 
        (FlexboxLayoutManager.LayoutParams) mImageView.getLayoutParams();
    flexboxLp.setFlexGrow(1.0f);
  }
}
```
无论屏幕方向如何，你都可以看到每一幅图像的布局都很适合。

![image](http://ol58plgkm.bkt.clouddn.com/flexboxlayoutmanager_demo_mode.gif)

如果你想查看完整的*FlexboxLayout*栗子，你可以检出：
- [Playground demo app](https://github.com/google/flexbox-layout/tree/dev_recyclerview/demo-playground) 使用*FlexboxLayout*和*FlexboxLayoutManager*
- [Cat gallery demo app](https://github.com/google/flexbox-layout/tree/dev_recyclerview/demo-playground) 使用*FlexboxLayoutManager*

### 接下来？
检出FlexboxLayout的其他属性相关[文档](https://github.com/google/flexbox-layout)。我们期望您的反馈，如果你发现任何问题或是有新功能需求，请在[Github库](https://github.com/google/flexbox-layout/issues)中提交。

### [原文链接](https://android-developers.googleblog.com/2017/02/build-flexible-layouts-with.html)