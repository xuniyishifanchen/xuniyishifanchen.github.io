---
title: "Repo"
date: 2023-04-19T00:09:20+08:00
draft: false

lightgallery: true

math:
  enable: true

tags: ["linux", "cmd"]
categories: ["linux cmd"]
---

# repo 命令

## 下载mirror 提高之后下载速度
```
repo init -u git:xxx/manifest -b branch –-mirror
```
## 通过mirror 下载代码, --depth 只下载最近的一次提交
```
repo init -u git:xxx/manifest -b branch –reference=path --depth 1
```
## 同步代码
```
repo sync -cj 32
```
## 切换所有分支到指定分支
```
repo start master --all
```
## 代码编译
```
1. source build/envsetup.sh
2. lunch (选择对应的机型)
3. ./build.sh fullbuild | tee build.log
```

## Android 编译环境配置
```
sudo apt-get install make
sudo apt install python-pip
pip install lxml
sudo apt-get install python-apt
sudo apt-get install python-debian
```
