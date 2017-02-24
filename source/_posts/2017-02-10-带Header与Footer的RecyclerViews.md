---
title: 带Header与Footer的RecyclerViews
date: 2017-02-10 15:54:09
categories:
- 技术
- Android
tags: 
- Android
- RecyclerView
---
## 前言
　使用过RecyclerView的朋友们，肯定对RecyclerView有了一定的了解，这个空间是google新出的一款用来代替ListView、GridView…列表View的新型控件,它不仅对性能优化提升了很多，而且更是增加了很多Item动画，在项目中使用它有非常不错的体验。 
     
　但是ListView中的一些功能在RecyclerView中并没有保留下来，其中就包括添加Header以及Footer的功能，其实网上已经有非常多的方法为RecyclerView添加Header以及Footer，我也看了其中几个的实现方式，但有代码洁癖的我不得不自己重写一套（一些是继承RecyclerView直接添加、还有是重写ViewHolder来添加） 
## 使用
 这里我是重写ViewHolder来实现这个功能，这样只要是RecyclerView，设置这个ViewHolder就能很简单的实现添加Header以及Footer的功能，下面贴上我自己写的一个简单的代码例子：
```
import android.support.v7.widget.RecyclerView;
import android.view.ViewGroup;

import java.util.List;

/**
 * **********************
 * Author: yu
 * Date:   2015/9/7
 * Time:   14:32
 * **********************
 */
public abstract class HeaderAdapters<T> extends RecyclerView.Adapter{

    /** 这两个常量用来设定默认的Header以及Footer的Item的ViewType */
    private static final int ITEM_TYPE_HEADER = -0x1;
    private static final int ITEM_TYPE_FOOTER = -0x2;

    protected List<T> list;

    public HeaderAdapters(List<T> list){
        this.list = list;
    }

    /**
     * 这里将onCreateViewHolder重写,因为已经设定好了Header以及Footer的Item的ViewType
     * ,首先判断viewType,如果是 ITEM_TYPE_HEADER 或 ITEM_TYPE_FOOTER 的话,说明该Item
     * 为Header或Footer,然后来做一些相关的操作,否则为普通的Item,而我已经提前写好了一个
     * 普通的Item onCreate的abstract方法,所以具体的普通Item的onCreateViewHolder实现放
     * 到了该方法中.
     */
    @Override
    public RecyclerView.ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        switch (viewType) {
            case ITEM_TYPE_HEADER:
                return onCreateHeaderHolder(parent);
            case ITEM_TYPE_FOOTER:
                return onCreateFooterHolder(parent);
            default:
                return onCreateItemViewHolder(parent, viewType);
        }
    }

    /**
     * 道理与重写onCreateViewHolder方法一样,不同之处在于该方法在父RecyclerView.Adapter
     * 中并没有返回viewType,所以得用其他方法来判定该Item是什么类型,这里我事先写好了两个
     * abstract方法（isHasHeader()以及isHasFooter()）,当需要添加Header以及Footer时,需要
     * 将这两个方法对应的返回true,然后在这个方法中,就可以根据position来判断Item的类型,同
     * onCreateItemViewHolder(),普通Item的onBindViewHolder也放到了onBindItemViewHolder
     * 中来事先
     */
    @Override
    public void onBindViewHolder(RecyclerView.ViewHolder holder, int position) {
        if (isHasHeader() && position == 0) {
            onBindHeaderViewHolder(holder);
        } else if (isHasFooter() && position == getItemCount() - 1) {
            onBindFooterViewHolder(holder);
        } else {
            onBindItemViewHolder(holder, getRealItemPosition(position));
        }
    }

    public abstract RecyclerView.ViewHolder onCreateItemViewHolder(ViewGroup parent, int viewType);

    /**
     * 当需要设定Header的布局时，重写该方法来返回一个HeaderHolder,这里的HeaderHolder只要
     * 继承自RecyclerView.ViewHolder即可.
     */
    public RecyclerView.ViewHolder onCreateHeaderHolder(ViewGroup parent) {
        return null;
    }

    /**
     * 当需要设定Footer的布局时，重写该方法来返回一个FooterHolder,这里的FooterHolder只要
     * 继承自RecyclerView.ViewHolder即可.
     */
    public RecyclerView.ViewHolder onCreateFooterHolder(ViewGroup parent) {
        return null;
    }

    public abstract void onBindItemViewHolder(RecyclerView.ViewHolder holder, int position);


    public void onBindHeaderViewHolder(RecyclerView.ViewHolder headerHolder) {

    }

    public void onBindFooterViewHolder(RecyclerView.ViewHolder footerHolder) {

    }

    /**
     * 获取真实的非Header以及Footer的Item的position
     */
    public int getRealItemPosition(int position) {
        return isHasHeader() ? position - 1 : position;
    }

    @Override
    public int getItemCount() {
        if (getList() != null) {
            return getList().size() + (isHasHeader() ? 1 : 0) + (isHasFooter() ? 1 : 0);
        }
        return 0;
    }

    /**
     * 获取真实的非Header以及Footer的Item的count
     */
    public int getRealItemCount() {
        return getList() != null ? getList().size() : 0;
    }

    /**
     * 是否添加Header,需要实现该方法并返回boolean值
     */
    public abstract boolean isHasHeader();

    /**
     * 是否添加Footer,需要实现该方法并返回boolean值
     */
    public abstract boolean isHasFooter();

    public List<T> getList(){
        return list;
    }
}
```
　具体的注释我在代码中已经写清楚了，其实这只是个我自己写的添加Header以及Footer的原型，在我自己的项目中，已经把其他需要的功能实现的更加完美，但不同项目有不同的需求，这里已经把那些功能删减掉了，有兴趣的朋友可以共同探讨下！
