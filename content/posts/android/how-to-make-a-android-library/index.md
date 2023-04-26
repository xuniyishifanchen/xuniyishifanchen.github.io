---
title: "如何构建自己的android lib库"
date: 2023-04-18T23:13:11+08:00
draft: false

lightgallery: true

math:
  enable: true

tags: ["android"]
categories: ["android"]
---

{{<youtube EzC-FXeZiIk>}}

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

### 配置gradle引用
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