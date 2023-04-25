---
title: "Android lib - Ktlint"
date: 2023-04-17T01:28:47+08:00
draft: false

tags: 
  - android
  - lib

categories:
  - android lib
---

## 1 About [Ktlint](https://github.com/JLLeitschuh/ktlint-gradle)
>一个kotlin代码格式化插件

{{<youtube hSgPNbEcX98>}}

## 2[非官方Android Studio插件](https://plugins.jetbrains.com/plugin/15057-ktlint-unofficial-)

## 3 配置
### 3.1 在`app`模块`build.gradle`中配置插件
```groovy
plugins {
    id 'com.android.application'
    ...
    id("org.jlleitschuh.gradle.ktlint") version "11.0.0"
}

```

### 3.2. 在`app`模块`build.gradle`配置使用参数
```groovy
ktlint {
    android = true
    ignoreFailures = false
    reporters {
    	reporter "plain"
    	reporter "checkstyle"
    	reporter "sarif"
    }
    disabledRules = ["final-newline", "other_xxx"]
    outputColorName.set("RED")
}

```

### 3.3. 运行检查代码格式错误
```shell
./gradlew ktlintCheck  
```

### 3.4. 自动格式化代码
```shell
./gradlew ktlintFormat  
```	
### 3.5. 在`app`模块`build.gradle`配置编译时自动格式化
```groovy
tasks.getByPath("preBuild").dependsOn("ktlintFormat")
```