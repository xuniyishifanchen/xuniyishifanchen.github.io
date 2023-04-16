---
title: "Ktlint"
date: 2023-04-17T01:28:47+08:00
draft: false

lightgallery: true

math:
  enable: true

tags: ["android", "lib"]
categories: ["android lib"]
---

# [Ktlint](https://github.com/JLLeitschuh/ktlint-gradle)

## [教学视频](https://www.youtube.com/watch?v=hSgPNbEcX98)

## [非官方插件](https://plugins.jetbrains.com/plugin/15057-ktlint-unofficial-)

## 1 配置
### 1.1 在gradle 中配置插件
```groovy
plugins {
    id 'com.android.application'
    ...
    id("org.jlleitschuh.gradle.ktlint") version "11.0.0"
}

```

### 1.2. 配置使用参数
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

### 1.3. 运行代码检查
```shell
./gradlew ktlintCheck  
```

### 1.4. 自动格式化
```shell
./gradlew ktlintFormat  
```	
### 1.5. gradle编译时自动格式化
```groovy
tasks.getByPath("preBuild").dependsOn("ktlintFormat")
```