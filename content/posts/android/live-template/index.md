---
title: "Android 快捷键模板"
date: 2023-04-18T22:28:18+08:00
draft: false

lightgallery: true

math:
  enable: true

tags: ["android", "快捷键"]
categories: ["android"]
---

# [Android 快捷键模板](https://gist.github.com/philipplackner/3d3b900d2e9128a37d7d5e6e8fac9e4f)
>Settings->Editor->Live Templates
{{< youtube HhBnNJqvzco >}}

## 1 `hiltvm` (applicable in top-level)
```kotlin
@dagger.hilt.android.lifecycle.HiltViewModel
class $NAME$ @javax.inject.Inject constructor(
$PARAM$
) : androidx.lifecycle.ViewModel() {
    $END$
}
```

## 2 `vmstate` (applicable in class)
```kotlin
private val _$NAME$ = androidx.compose.runtime.mutableStateOf<$TYPE$>($INITIAL_VALUE$)
val $NAME$: androidx.compose.runtime.State<$TYPE$> = _$NAME$
```

## 3 `vmflow` (applicable in class)
```kotlin
private val _$NAME$ = kotlinx.coroutines.flow.MutableStateFlow<$TYPE$>($INITIAL_VALUE$)
val $NAME$ = _$NAME$.asStateFlow()
```

## 4 `vmsharedflow` (applicable in class)
```kotlin
private val _$NAME$ = kotlinx.coroutines.flow.MutableSharedFlow<$TYPE$>()
val $NAME$ = _$NAME$.asSharedFlow()
```

## 5 `remstate` (applicable in Kotlin except Comment)
```kotlin
var $NAME$ by androidx.compose.runtime.remember {
    androidx.compose.runtime.mutableStateOf($INITIAL_VALUE$)
}
```

## 6 `centerbox` (applicable in Kotlin except Comment)
```kotlin
androidx.compose.foundation.layout.Box(
    modifier = androidx.compose.ui.Modifier.fillMaxSize(),
    contentAlignment = androidx.compose.ui.Alignment.Center
) {
    $END$
}
```

## 7 `iconbtn` (applicable in Kotlin except Comment)
```kotlin
androidx.compose.material.IconButton(
    onClick = {
    
    },
) {
    androidx.compose.material.Icon(
        imageVector = $ICON$,
        contentDescription = $CONTENT_DESCRIPTION$
    )
}
```