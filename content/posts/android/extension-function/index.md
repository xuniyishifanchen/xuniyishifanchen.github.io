---
title: "Extension Function"
date: 2023-04-18T23:23:12+08:00
draft: false

lightgallery: true

math:
  enable: true

tags: ["android", "扩展函数"]
categories: ["android"]
---

# 实用的扩展函数

```kotlin
fun <T> ComponentActivity.collectLatestLifecycleFlow(flow: Flow<T>, collect: suspend (T) -> Unit) {
    lifecycleScope.launch {
        repeatOnLifecycle(Lifecycle.State.STARTED) {
            flow.collectLatest(collect)
        }
    }
}
```