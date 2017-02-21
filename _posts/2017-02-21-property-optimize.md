---
layout: post
title:  "Android 我见知 - 性能优化"
date:   2017-02-21 13:47:00 +0800
categories: Android
tag: Android 我见知
excerpt: 
---

* content
{:toc}

## 布局优化
### Android UI渲染机制
要保持画面的流畅，需要保证画面的帧率达到40fps每秒到60fps每秒。系统通过VSYNC信号触发对UI的渲染，重绘，期间隔时间为16ms，如果系统每次渲染的间隔都保持在16ms，那么我们所看到的界面基本流畅。如果不能保证在16ms之内渲染完成，那么就会照常丢帧现象。

Android提供了检测UI渲染时间的工具，在开发者选项中选择Profile Rendering，选择On screen bars 。 蓝色代表测量绘制Display List的时间， 红绳代表OpenGL渲染时间，还死代表GPU处理时间，绿色代表VSYNC时间16ms。

### 避免过度绘制Overdraw
过度绘制会浪费很多的CPU，GPU资源
在开放者选项中使用 Enable GPU OverDraw 来检查，优化布局，增大蓝色区域，减少红色区域。
 
### 优化布局层级
降低View树的高度，Google在API文档中建议View树高度不宜超过10层。

### 避免嵌套过多无用布局
使用<include>标签重用Layout。<br/>
使用<ViewStub>实现View的延迟加载

### Hierarchy Viewer
这个只有非加密过的手机可以使用，加密手机可以使用开源项目 View Server ，然后进行分析


## 内存优化
### 什么是内存
[请点击]()

### 获取Android系统内存信息
通过setting - Developer options - Process Stats打开。

### 内存回收
Java是由系统自动回收的（GC），但是合适进行开发者无法控制，即使调用System.gc()方法，也只是建议系统GC。

### 内存优化
- Bitmap优化
    - 即使回收内存
    - 使用图片缓存


- 代码优化
    - 对常量使用static修饰符。
    - 使用静态方法，静态方法比普通方法提高15%左右的访问速度。
    - 减少不必要的成员变量。
    - 减少不必要的对象。
    - 尽量不要使用枚举。
    - 对Cursor、Receiver、Sensor、File等对象，要非常注意对他们的创建、回收、注册，解注册。
    - 避免使用IOC框架，IOC通常使用注解，反射来进行实现，虽然Java对反射的效率已经进行了很好的优化，但是大量使用反射依然会带来性能下降问题。
    - 使用RenderScript、OpenGL来进行非常复杂的绘图操作。
    - 使用SurfaceView来替代View进行大量、频繁的绘图操作
    - 尽量使用视图缓存，而不是每次都执行inflate()方法解析视图。


## Lint工具
eclipse和Android studio都集成自带。


## 使用AS的Memory Monitor工具

## 使用TraceView工具

## 使用MAT工具