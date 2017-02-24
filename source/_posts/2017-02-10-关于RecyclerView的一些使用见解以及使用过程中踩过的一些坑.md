---
title: 关于RecyclerView的一些使用见解以及使用过程中踩过的一些坑
date: 2017-02-10 11:27:53
categories:
- 技术
- Android
tags: 
- Android
- RecyclerView
---
## 前言
&#8195;&#8195;说到RecyclerView，大家肯定再熟悉不过了。所以呢，这里就不啰里啰嗦介绍一大堆关于RecyclerView的知识点了，这篇文章呢，主要介绍我在开发过程中使用到RecyclerView总结的一些经验以及踩过的一些坑。  
&#8195;&#8195;首先先说下RecyclerView的一些新特性，从SupportLibrary V23.2开始，Google给RecyclerView的LayoutManager添加了新特性自动测量（auto-measurement），它允许RecyclerView根据内容自动控制高度，这意味着什么呢？回想一下以前的一些应用场景，有这样一个需求，需要一个列表页，列表页某个部位还需要有一个列表页，如果是以前，我们可能会这么做：1.外层先放一个ScrollView，然后在里层放两个ListView，然后重写ListView的onMeasure方法，达到ListView适应ScrollView的效果；2.使用RecyclerView或ListView，然后在它的头布局或其他某个布局中再嵌套RecyclerView或ListView；3.使用ScrollView嵌套LinearLayout，然后在LinearLayout中动态的添加Item以达到复杂的布局效果。这样无疑增加了代码的复杂度以及工作量，而且有时候体验效果还有点不尽人意。但自从有了23.2的这个新特新后，我们可以通过RecyclerView之间或RecyclerView与其他View的互相嵌套实现各种复杂的布局，而且并不用担心滚动适配等问题。

来看下效果：  

![image](http://ol58plgkm.bkt.clouddn.com/img_2017_02_10_01.gif)

## 使用起来其实很简单
首先建好父布局，放一个RecyclerView即可：
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <android.support.v7.widget.Toolbar
        android:id="@id/toolbar"
        style="@style/Test.Toolbar"/>

    <android.support.v7.widget.RecyclerView
        android:id="@id/recycler_view"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        />

</LinearLayout>
```
然后是头布局，这里在头布局中再放入一个RecyclerView。
```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical">

    <android.support.v7.widget.RecyclerView
        android:id="@+id/header_recycler_auto_recycler"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        />

    <ImageView
        android:layout_width="match_parent"
        android:layout_height="@dimen/size_300"
        android:background="#ffff00"
        android:scaleType="centerCrop"
        android:src="@drawable/img_1"
        />
</LinearLayout>
```
接着是Item布局

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:gravity="left|center"
    android:orientation="horizontal"
    android:padding="@dimen/size_10">

    <ImageView
        android:layout_width="@dimen/size_40"
        android:layout_height="@dimen/size_40"
        android:src="@drawable/ic_whale"
        />

    <TextView
        android:id="@+id/item_recycler_auto_text"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:paddingLeft="@dimen/size_20"
        android:textColor="#333333"
        />
</LinearLayout>
```

最后是Activity代码

```
public class RecyclerAutoActivity extends ParentActivity {

    protected RecyclerView mRecycler;
    protected ItemAdapter mAdapter;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        super.setContentView(R.layout.activity_recycler_auto);
        initView();
    }

    private void initView() {
        mRecycler = (RecyclerView) findViewById(R.id.recycler_view);
        mRecycler.setLayoutManager(new LinearLayoutManager(getContext()));
        mRecycler.addItemDecoration(new LinearItemDecoration(getContext().getResources().getDimensionPixelOffset(R.dimen.size_1), Color.parseColor("#eeeeee")));
        mRecycler.setAdapter(mAdapter = new ItemAdapter(getContext()));
        List<String> tempList = new ArrayList<>();
        for (int i = 0; i < 10; i++) {
            tempList.add("This is content item " + i);
        }
        mAdapter.insertRange(tempList, false);
    }

    class ItemAdapter extends HeaderAdapter<String> {

        public ItemAdapter(Context context) {
            super(context);
        }

        @Override
        public boolean isHasHeader() {
            return true;
        }

        @Override
        public ViewHolderPlus<String> onCreateHeaderHolder(ViewGroup parent, LayoutInflater inflater) {
            return new HeaderHolder(createView(R.layout.header_recycler_auto, parent));
        }

        @Override
        public ViewHolderPlus<String> onCreateItemViewHolder(ViewGroup parent, int viewType, LayoutInflater inflater) {
            return new ItemHolder(createView(R.layout.item_recycler_auto, parent));
        }

        class HeaderHolder extends ViewHolderPlus<String> {

            RecyclerView mRecycler;
            ItemHeaderAdapter mAdapter;

            public HeaderHolder(View itemView) {
                super(itemView);
                mRecycler = (RecyclerView) itemView.findViewById(R.id.header_recycler_auto_recycler);
                mRecycler.setNestedScrollingEnabled(false);
                mRecycler.setLayoutManager(new GridLayoutManager(getContext(), 3));
                mRecycler.addItemDecoration(new GridItemDecoration(3, getContext().getResources().getDimensionPixelOffset(R.dimen.size_10)));
                mRecycler.setAdapter(mAdapter = new ItemHeaderAdapter(getContext()));
            }

            @Override
            public void onBinding(int position, String s) {
                List<String> tempList = new ArrayList<>();
                for (int i = 0; i < 10; i++) {
                    tempList.add("This is header item " + i);
                }
                mAdapter.clear();
                mAdapter.insertRange(tempList, false);
            }

            class ItemHeaderAdapter extends AdapterPlus<String> {

                public ItemHeaderAdapter(Context context) {
                    super(context);
                }

                @Override
                public ViewHolderPlus<String> onCreateViewHolder(ViewGroup parent, int viewType, LayoutInflater inflater) {
                    return new ItemHolder(createView(R.layout.item_recycler_auto, parent));
                }
            }
        }

        class ItemHolder extends ViewHolderPlus<String> {

            protected TextView mText;

            public ItemHolder(View itemView) {
                super(itemView);
                initView(itemView);
            }

            @Override
            public void onBinding(int position, String s) {
                mText.setText(s);
            }

            private void initView(View rootView) {
                mText = (TextView) rootView.findViewById(R.id.item_recycler_auto_text);
            }
        }
    }
}
```
整个页面大概的布局为RecyclerView添加一个头布局，然后在头布局中再添加一个RecyclerView以及一个ImageView，可以看到展示效果完全没问题。   


## 需要注意的一些坑：
1.RecyclerView中Adapter的Item布局不能设置layout_height="match_parent"，否则会出现以下情况，每个Item的高度都铺满了屏幕：   
![image](http://ol58plgkm.bkt.clouddn.com/img_2017_02_10_02.gif)   

2.当RecyclerView多层嵌套时，需要在最外层的父布局中加入下面代码，否则可能会出现从其他页面返回该页面时，view自动滚动到其中某个RecyclerView布局顶部的现象:

```
android:focusable="true"
android:focusableInTouchMode="true"
```
3.当RecyclerView多层嵌套且外层使用有AppbarLayout时，需要在子RecyclerView的初始化代码中加入下面代码，否则会出现滚动RecyclerView时，只有RecyclerView部分局部滚动的现象：

```
mRecyclerView.setNestedScrollingEnabled(false);
```
![image](http://ol58plgkm.bkt.clouddn.com/img_2017_02_10_03.gif)

