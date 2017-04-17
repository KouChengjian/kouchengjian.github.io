---
layout: post
title:  "Android开发艺术探 第七章 Android动画深入分析"
date:   2017-04-12 11:21:00 +0800
categories: Android
tag: Android开发艺术探
excerpt: Android动画可以分成三种：View动画，帧动画，属性动画。帧动画也属于View动画。View动画只能改变现实效果，不能真正改变View的属性。
---

* content
{:toc}

* [View动画](#View动画)
    * [View动画的种类](#View动画的种类)
    * [自定义View动画](#自定义View动画)
    * [帧动画](#帧动画)
* [View动画特殊使用场景](#View动画特殊使用场景)
    * [LayoutAnimation](#LayoutAnimation)
    * [Activity的切换效果](#Activity的切换效果)
* [属性动画](#属性动画)
    * [使用属性动画](#使用属性动画)
    * [理解插值器和估值器](#理解插值器和估值器)
    * [属性动画的监听器](#属性动画的监听器)
    * [对任意属性做动画](#对任意属性做动画)
    * [属性动画的工作原理](#属性动画的工作原理)
* [使用动画的注意事项](#)

# View动画
## View动画的种类

| 名称   | 标签 | 子类 | 效果 |
| ----  |-----|--------|------|
| 平移动画 | < translate > | TranslateAnimation |移动View|
| 缩放动画 | < scale > | ScaleAnimation |放大或者缩小View|
| 旋转动画 |  < rotate >  | RotateAnimation |旋转View|
| 透明度动画 | < alpha >  | AlphaAnimation|改变View的透明度|

< set > 标签标示动画合集，对应AnimationSet类，它可以包含诺干个动画，并且它的内部也是可以嵌套其它动画集合的，它的两个属性含义：
1、android:interpolator  标示动画插值器，插值器影响动画速度，比如非匀速动画就要通过插值器来控制。

2、android:ShareInterpolator  标示集合的动画是否和集合共享同一个插值器。

## 自定义View动画
派生一种新动画只需要继承Animation这个抽象类，然后重写initialize和applyTransformation方法，在initalize中初始化工作，在applyTransformation进行相应的矩阵变化，很多时候需要使用Camera来简化矩阵变化过程。
## 帧动画
帧动画是顺序播放一组预先定义好的图片，类似于电影播放。
# View动画特殊使用场景
## LayoutAnimation
LayoutAnimation作用于ViewGroup，为ViewGroup指定一个动画，这样当它的子元素出场时都会有这种动画，常用语ListView。
## Activity的切换效果
Activity切换效果，主要用到overriderPendingTransition(int enterAnim , int exitAnim),这个方法必须在startActivity(Intent)或者finish()之后被调用才能生效。

* enterAnim - Activity打开时，所调用动画资源的id。
* exitAnim  - Activity暂停时，所调用动画资源id。

# 属性动画
属性动画是API11新加入的特性，和View动画不同，它对作用对象进行了扩展，属性动画可以对任何对象做动画，甚至可以没有对象。
## 使用属性动画
属性动画可以对任意对象属性进行动画而不仅仅是View，动画默认时间间隔300ms，默认帧率10ms/帧。版本API11以下可以使用nineoldAndroid。

(1)、改变一个对象（myobjeck）的translationY属性，让它沿着Y轴向上平移一段距离。

    ObjeckAnimator.ofFloat(myObjeck,"translationY",-myObjeck.getHeight());
    
(2)、改变一个对象的背景属性，典型的是改变View的背景颜色。

    ValueAnimator colorAnim = ObjeckAnimator.ofInt(this,"backgroundColor",/*Red*/0XFFFF8080, /*blue*/0xFF8080FF);
    colorAnim.setDuration(3000);
    colorAnim.setEvaluator(new ArgbEvaluator());
    colorAnim.setRepeatCount(ValueAnimator.INFINITE);
    colorAnim.setRepeatMode(ValueAnimator.REVERSE);
    colorAnim.start();
    
(3)、动画集合，5秒对View的旋转，平移，缩放，透明度进行改变。
    
    AnimatorSet set = new AnimatorSet();
    set.playTogether(
        ObjectAnimator.ofFloat(myView,"rotationx",0,360);
        ObjectAnimator.ofFloat(myView,"rotationY",0,180);
        ObjectAnimator.ofFloat(myView,"rotation",-90);
        ObjectAnimator.ofFloat(myView,"translationX",0,90);
        ObjectAnimator.ofFloat(myView,"translationY",0,90);
        ObjectAnimator.ofFloat(myView,"scaleX",1,1.5f);
        ObjectAnimator.ofFloat(myView,"scaleY",1,0.5f);
        ObjectAnimator.ofFloat(myView,"",1,0.25f,1);
    );
    set.setDuration(5*1000).start();
    
## 理解插值器和估值器
TimeInterpolator为时间插值器，它的作用是根据时间流逝的百分比来计算当前属性改变的百分比，系统预制有LinearInterpolator（线性插值器：匀速动画），AccelerateDecelerateInterpolator（加速减速插值器：动画两头慢中间快）和DecelerateInterpolator（减速插值器：动画越来越慢）等。

TypeEvaluator为类型估值算法，也叫估值器，它的作用是根据当前属性改变的百分比来计算改变后的属性。
## 属性动画的监听器
1、AnimatorListener 主要监听动画开始，结束，取消以及重复播放。
2、AnimatorUpdateListener 监听动画整个过程，动画没播放一帧，就回被调用一次。
## 对任意属性做动画
1、给你的对象加上get、set方法，如果你有权限的话。
2、用一个类来包装原始对象，间接提供get和set方法。
3、采用VaueAnimator，监听动画过程，自己实现属性的改变
## 属性动画的工作原理
# 使用动画的注意事项
1、OOM问题

2、内存泄漏问题

3、兼容问题

4、View动画问题

5、不要使用PX

6、动画元素的交互

7、硬件加速

    使用动画的过程，建议开启硬件加速，会使动画流畅。