---
title: 微信小程序开发iPhone底部因为横杠适配有问题
date: '2024-12-22'
categories: 
  - [前端开发, 微信小程序]
  - [问题解决, UI适配]
tags: ['微信小程序', '前端开发']
description: 微信小程序开发iPhone底部因为横杠适配问题解决
keywords: 微信小程序, iPhone, 横杠适配, 前端开发
---

在做一个商城小程序开发的时候遇到的一个问题，有些 iphone 手机最下边有一个横条，就像这样：

![wechatminiapp](images/wechat-miniapp/wechatminiapp.png)

当我们正常开发的时候就会变成：

![wechatminiapp2](images/wechat-miniapp/wechatminiapp2.png)

如果加一条 margin-bottom 适配就变得没问题了，但是如果在没有这条横杠的手机上就会变得很奇怪：

![wechatminiapp3](images/wechat-miniapp/wechatminiapp3.png)

解决办法：

​		增加一条 css 样式 `padding-bottom: env(safe-area-inset-bottom);`

​		这用于设置元素的底部内边距。这里的`env()`函数是一个CSS环境变量函数，它允许你根据设备的特定环境特性来设置样式值。

`safe-area-inset-bottom`是一个特定的环境变量，它返回的是设备屏幕底部安全区域的内边距值。安全区域是指在屏幕边缘（如刘海、圆角、传感器等）不会遮挡内容的区域。在一些设备上，尤其是带有刘海或圆角的手机屏幕上，内容可能会被这些屏幕特征遮挡，因此开发者需要避免在这些区域放置重要内容。

使用`env(safe-area-inset-bottom)`的好处是，它能够自动适应不同设备的屏幕形状和尺寸，确保内容不会被遮挡。这样，开发者就不需要硬编码一个固定的值，而是可以动态地根据设备的安全区域来调整布局。

简而言之，`padding-bottom: env(safe-area-inset-bottom);` 意味着元素的底部内边距将等于设备底部安全区域的内边距，从而确保内容不会被屏幕底部的任何遮挡物（如刘海）遮挡。