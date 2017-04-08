---
layout: post
title: "Android开发艺术探 第一章 Android生命周期和启动模式"
date: 2017-04-07 19:45:00 +0800
categories: Android
tag: Android开发艺术探
<!-- excerpt: -->
---

* content
{:toc}

* [Android的生命周期和启动模式](#Android的生命周期和启动模式)
    * [正常情况生命周期分析](#正常情况生命周期分析)
    * [异常情况生命周期分析](#异常情况生命周期分析)
* [Activity的启动模式](#Activity的启动模式)
    * [Activity的LaunchMode](#Activity的LaunchMode)
    * [Activity的Flags](#Activity的Flags)
* [IntentFilter的匹配 规则](#IntentFilter的匹配规则)



# Android的生命周期和启动模式
## 正常情况生命周期分析
(1)第一次启动Activity，回调如下：onCreate -> onStart -> onResume。

(2)当打开新的Activity或者切换到桌面时，回调如下：onpause -> onStop。如果新Activity是透明主题时，旧Activity不会走onStop。

(3)当用户在回到原Activity时，回调如下：OnRestart -> onStart -> onResume。

(4)当用户安心break时，回调如下： onPause -> onStop -> onDestroy。

(5)onStart和onPause是从是否可见这个角度来回调的，onResume和onStop是从Activity是否位于前台这个调度回调的

(6) onStart和onResume的区别是onStart可见，还没有出现在前台，无法和用户进行交互。onResume获取到焦点可以和用户交互。

(7)Activity切换时，旧Activity的onPause会先执行，然后才会启动新的Activity；
 

## 异常情况生命周期分析
Activity在异常情况下被回收时，onSaveInstanceState方法会被回调，回调时机是在onStop之前，当Activity被重新创建的时候，onRestoreInstanceState方法会被回调，时序在onStart之后；

# Activity的启动模式
## Activity的LaunchMode
a. standard 系统默认。每次启动会重新创建新的实例，谁启动了这个Activity，这个Activity就在谁的栈里。

b. singleTop 栈顶复用模式。该Activity的onNewIntent方法会被回调，onCreate和onStart并不会被调用。

c. singleTask 栈内复用模式。只要该Activity在一个栈中存在，都不会重新创建，onNewIntent会被回调。如果不存在，系统会先寻找是否存在需要的栈，如果不存在该栈，就创建一个任务栈，然后把这个Activity放进去；如果存在，就会创建到已经存在的这个栈中。

d. singleInstance。具有此种模式的Activity只能单独存在于一个任务栈。
## Activity的Flags
常用的：

FLAG_ACTIVITY_SINGLE_TOP = singleTop<br/>

FLAG_ACTIVITY_NEW_TASK = singleTask<br/>

FLAG_ACTIVITY_CLEAR_TOP : 一般会和singleTask一起出现，如果Activity采用standard启动，那边会清除本身以及之上的所有Activity，重新创建，并且押入栈顶。


# IntentFilter的匹配规则

a. action匹配规则：要求intent中的action存在且必须和过滤规则中的其中一个相同 区分大小写；

b. category匹配规则：系统会默认加上一个android.intent.category.DEAFAULT，所以intent中可以不存在category，但如果存在就必须匹配其中一个；

c. data匹配规则：data由两部分组成，mimeType和URI，要求和action相似。如果没有指定URI，URI但默认值为content和file（schema）



