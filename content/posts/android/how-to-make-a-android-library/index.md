---
title: "How to Make a Android Library"
date: 2023-04-18T23:13:11+08:00
draft: false

lightgallery: true

math:
  enable: true

tags: ["android"]
categories: ["android lib"]
---

# 构建自己的android lib库

[教学视频](https://www.youtube.com/watch?v=EzC-FXeZiIk)

## 1. 创建自己的android module

## 2. 配置gradle文件

```groovy
plugins {
    ...
    id 'maven-publish'
}

afterEvaluate {
    publishing {
        publications {
            release(MavenPublication) {
                from components.release

                groupId = 'com.github.Healwy'
                artifactId = 'image-preview-compose'
                version = '1.0'
            }
        }
    }
}
```

## 3. 上传到 https://jitpack.io/

# 如何使用

## 1. 配置gradle引用
``` groovy
	allprojects {
		repositories {
			...
			maven { url 'https://jitpack.io' }
		}
	}
```

```groovy
implementation 'com.github.User:Repo:Tag'
```