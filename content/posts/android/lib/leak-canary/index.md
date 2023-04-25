---
title: "Android lib - Leak Canary"
date: 2023-04-18T01:28:22+08:00
draft: false

tags: 
  - android
  - lib

categories:
  - android lib
---

## 1 About [LeakCanary](https://square.github.io/leakcanary)
>LeakCanary 是大名鼎鼎的 square 公司开源的内存泄漏检测工具。目前上大部分App在开发测试阶段都会接入此工具用于检测潜在的内存泄漏问题，做的好一点的可能会搭建一个服务器用于保存各个设备上的内存泄漏问题再集中处理。

{{<youtube VvkRe9vP5Oc>}}

## 2. 使用方法
添加leakcanary-android 依赖到app的`build.gradle`文件即可。
```groovy
dependencies {
  // debugImplementation because LeakCanary should only run in debug builds.
  debugImplementation 'com.squareup.leakcanary:leakcanary-android:2.8.1'
}
```

## 3. 分析内存泄漏
分析方法：<https://square.github.io/leakcanary/fundamentals-how-leakcanary-works/>

github主页：<https://github.com/square/leakcanary>