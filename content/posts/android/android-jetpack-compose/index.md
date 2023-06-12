---
title: "Android Jetpack Compose"
date: 2023-06-10T22:27:23+08:00
draft: false

tags: 
  - android
  - jetpack compose

categories:
  - android 
  - jetpack compose
---

# 1.[Jetpack Compose](https://developer.android.com/jetpack/compose/documentation?hl=zh-cn)

## `staticCompositionLocalOf`和`compositionLocalOf`的区别是什么？
>如果使用compositionLocalOf来创建CompositionLocal，那么当状态更新时，只有其内部用了CompositionLocal的current数值的Composeable才会重组，而使用staticCompositionLocalOf的话，其所包含的所有内容都会重组，由于跟踪使用CompositionLocal的current的Composeable需要消耗资源，所以建议如果CompositionLocal的current更改的可能性很小，则建议使用staticCompositionLocalOf。

## Side Effect

### LaunchedEffect
>如果在可组合函数中执行耗时操作，就需要将耗时操作放到协程中执行，而协程需要再协程作用域中创建，因此Compose提供了LaunchedEffect用于创建协程。
- 当LaunchedEffect进入组件树时，会启动一个协程，并将block放入该协程执行。
- 当组合函数重组件树上detach时，协程还未执行完毕，则协程也会`被取消执行`。
- 当LaunchedEffect在重组时其key不变，那LaunchedEffect不会被重启执行block。
- 当LaunchedEffect在重组是key发生了变化，则LaunchedEffect会执行cancel后，在重新启动一个新协程执行block。

### rememberCoroutineScope
>由于LaunchedEffect是可组合函数，因此只能在其他可组合函数中使用。想要在可组合项外启动协程，且需要对这个协程作用域限制，以便协程在退出组合后自动取消，可以使用rememberCoroutineScope。
>此外，如果需要手动控制一个或多个协程的生命周期，请使用rememberCoroutineScope。

### rememberUpdatedState
>如果key值有更新，那么LaunchedEffect重组时就会重新启动，但是有时候需要再LaunchedEffect中使用最新的参数值，但是又不想重新启动LaunchedEffect，此时就需要用到rememberUpdatedStatecc。
>rememberUpdatedState的作用是给某个参数创建一个引用，来跟踪这些参数，并`保证其值被使用时是最新值，参数被改变时不重启effect`。

### DisposableEffect
>DisposableEffect也是一个可组合函数，当DisposableEffect的key变化或者组合函数离开组件树的时候，会取消之前的协程，并会在取消协程前调用其回收方法进行资源回收相关的操作，可以对一些资源进行处理。


### SideEffect
>SideEffect是简化版的DisposableEffect，SideEffect并未接收任何key值，所以，每次重组，都会执行其block，当不需要onDispose，不需要参数控制时使用SideEffet。
>SideEffect主要用来与非Compose管理的对象共享Compose状态。
>SideEffect在组合函数创建并载入视图树后才会被调用。

### produceState
>将非Compose（如Flow，LiveData或Rxjava）状态转换为Compose状态。它接收一个lambda表达式作为函数体，能将这些入参经过一些操作后生成一个State类型变量返回。
>produceState创建一个协程，但它也可用于观察非挂起的数据源。
>当produceState进入Composition时，获取数据的任务会被启动，当期离开Composition时，该任务被取消。

### derivedStateOf
>如果某个状态是从其他状态对象计算或派生出来的，请使用derivedStateOf。作为条件的状态我们称为条件状态。当任意一个条件状态更新时，结果都会重新计算。

### snapshotFlow
>使用snapshotFlow可以将state转换成Flow。snapshotFlow会传入运行的block，并发出从块中读取State对象的结果。当snapshotFlow快中读取的State对象之一发生变化时，如果新值与之前发出的值不相等，Flow会向其收集器发出新值。