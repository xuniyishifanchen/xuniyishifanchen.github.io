---
title: "Kotlin Coroutine"
date: 2023-04-18T22:37:34+08:00
draft: false

lightgallery: true

math:
  enable: true

tags: ["language", "kotlin"]
categories: ["language"]
---

# 协程

## 教学网站
1. https://www.bennyhuo.com/book/kotlin-coroutines/

## 教学视频
1. https://space.bilibili.com/28615855/channel/collectiondetail?sid=375593
2. https://www.bilibili.com/video/BV1uo4y1y7ZF?spm_id_from=333.337.search-card.all.click

## 教学书籍
[深入理解kotlin协程](/pdf/深入理解Kotlin协程_霍丙乾.pdf) 

## 协程作用域：
它会跟踪所有协程，同样他也可以取消由他启动的所有协程。

1. `GlobalScope`: 生命周期是process级别的，即使Activity或Fragment已经被销毁，协程仍然在执行。
2. `MainScope`: 在Activity 中使用，可以在onDestory中取消协程。
3. `viewModelScope`: 只能在ViewModel中使用，绑定viewModel的生命周期
4. `lifeCycleScope`: 只能在Activity, Fragment中使用，绑定Activity, Fragment的生命周期

## 协程构建器
1. `launch`：返回一个job并且不附带任何结果。
2. `async`：返回一个Deferred, Deferredy也是一个job可以使用await()在一个延期的值上得到他的最终结果。

## 协程作用域构建器
1. `coroutineScope`: 一个协程失败了，其他的所有兄弟协程也会被取消。
2. `supervisorScope`: 一个协程失败了，不会影响其他兄弟协程。

## 协程的取消
1. 取消作用域会取消他的子协程。
2. 被取消的子协程并不会影响其余兄弟协程。
3. 协程通过抛出一个特殊的异常CancellationException来处理取消操作。
4. 所有kotlin.coroutines中的挂起函数(withContext, delay等)都是可以取消的。

## Cpu密集任务协程取消
1. isActive是一个可以被使用在CoroutineScope中的扩展属性，检查job是否处于活跃状态。
2. ensureActive(), 如果job处于非活跃的状态，这个方法会立即抛出异常。
3. yield函数会检查所在协程的状态，如果已经取消，则抛出CancellException予以响应。此外，它还会尝试出让线程的执行权，给其他协程提供执行的机会。

## 协程启动模式
1. `DEFAULT`: 协程创建后，立即开始调度，在调度前如果协程被取消，其将进入取消响应状态。
2. `ATOMIC`: 协程创建后，立即开始调度，协程执行到第一个挂起点之前协程不能被取消。
3. `LAZY`: 只有协程被需要时，包括主协程调用协程的start, join或者await等函数时才会开始调度，如果调度前就被取消，那么协程将直接进入异常结束状态。
4. `UNDISPATCHED`: 协程创建后立即在当前函数调用栈中执行，知道遇到第一个真正的挂起点。
