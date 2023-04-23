---
title: "Android lib - Spotless"
date: 2023-04-17T01:28:48+08:00
draft: false

lightgallery: true

math:
  enable: true

tags: ["android", "lib"]
categories: ["android lib"]
---

## 1 About [Spotless](https://github.com/diffplug/spotless)
> 代码格式化插件，包含各种语言

## 2 配置 
### 3.1 在根目录gradle.kts中配置插件及使用参数
```kotlin
plugins {
    id 'com.android.application'
    ...
    id("com.diffplug.spotless") version "6.15.0"
}

```

```kotlin
allprojects {
    // spotless 参数配置
    apply(plugin = "com.diffplug.spotless")
    configure<com.diffplug.gradle.spotless.SpotlessExtension> {
        // kotlin 配置
        kotlin {
            target("**/*.kt")
            targetExclude("$buildDir/**/*.kt", "spotless/*")
            ktlint("0.48.2")
                .userData(mapOf("android" to "true"))
                .editorConfigOverride(
                    mapOf(
                        "ij_kotlin_allow_trailing_comma" to "false",
                        "ij_kotlin_allow_trailing_comma_on_call_site" to "false"
                    )
                )
            licenseHeaderFile(rootProject.file("spotless/copyright.kt"))
        }
        // java 配置
        java {
            target("**/*.java")
            targetExclude("$buildDir/**/*.java", "spotless/*")
            googleJavaFormat("1.8").aosp()
            licenseHeaderFile(rootProject.file("spotless/copyright.java"))
        }
        // kts 配置
        kotlinGradle {
            target("**/*.kts")
            targetExclude("$buildDir/**/*.kts", "spotless/*")
            ktlint("0.48.2")
                .editorConfigOverride(
                    mapOf(
                        "ij_kotlin_allow_trailing_comma" to "false",
                        "ij_kotlin_allow_trailing_comma_on_call_site" to "false"
                    )
                )
            licenseHeaderFile(rootProject.file("spotless/copyright.kts"), "(^(?![\\/ ]\\*).*$)")
        }
        // xml 配置
        format("xml") {
            target("**/*.xml")
            targetExclude("**/build/**/*.xml", "spotless/*")
            licenseHeaderFile(rootProject.file("spotless/copyright.xml"), "(<[^!?])")
        }
    }

    // Configure Android projects
    pluginManager.withPlugin("com.android.application") {
        configureAndroidProject()
    }
    pluginManager.withPlugin("com.android.library") {
        configureAndroidProject()
    }
    pluginManager.withPlugin("com.android.test") {
        configureAndroidProject()
    }
}

fun Project.configureAndroidProject() {
    extensions.configure<com.android.build.gradle.BaseExtension> {
        tasks.getByPath("preBuild").dependsOn("spotlessApply")
    }
}
```

头文件配置
> 在根目录创建spotless目录，并放置各种语言的头文件

{{< admonition example>}}
#### copyright.java
```
/*
 * Copyright $YEAR Healer Corporation.
 */
 
 
```
#### copyright.kt
```
/*
 * Copyright $YEAR Healer Corporation.
 */
 
 
```
#### copyright.kts
```
/*
 * Copyright $YEAR Healer Corporation.
 */

 
```
#### copyright.xml
```
<?xml version="1.0" encoding="utf-8"?>
<!--
  ~ Copyright $YEAR Healer Corporation.
  -->
 
 
```
{{< /admonition >}}
### 3.3. 运行检查代码格式错误
```shell
./gradlew spotlessCheck  
```

### 3.4. 自动格式化代码
```shell
./gradlew spotlessApply  
```	