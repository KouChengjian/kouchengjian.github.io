---
layout: post
title:  "Android开发艺术探 第十章 Android的消息机制"
date:   2017-04-18 11:20:00 +0800
categories: Android
tag: Android开发艺术探
excerpt: Android的消息机制主要指Handler的运行机制，Handler的运行需要底层的MessageQueue和Looper的支撑。
---

* content
{:toc}

* [Android消息机制概述](Android消息机制概述)
* [Android消息机制分析](Android消息机制分析)
    * [ThreadLocal的工作原理](ThreadLocal的工作原理)
    * [消息队列的工作原理](消息队列的工作原理)
    * [Looper的工作原理](Looper的工作原理)
    * [Handle的工作原理](Handle的工作原理)
* [主线程的消息循环](主线程的消息循环)

# Android消息机制概述
Android的消息机制主要指Handler的运行机制以及Handler所附带的MesssageQueue和Loop而的工作过程。

# Android消息机制分析
## ThreadLocal的工作原理
ThreadLocal是一个线程内部的数据存储类，通过它可以在指定的线程中存储数据，数据存储之后，只有在指定线程中可以获取到存储数据。


## 消息队列的工作原理
## Looper的工作原理
## Handle的工作原理
# 主线程的消息循环