---
title: "Linux Software - 常用软件"
date: 2023-04-19T00:06:17+08:00
draft: false

lightgallery: true

math:
  enable: true

tags: ["software", "linux"]
categories: ["linux software"]
---

## GNOME Tweaks 显示周数的软件
```
sudo apt-get install gnome-tweaks
```
{{< admonition tip>}}
设置显示周数: info -> Top bar -> week number
{{< /admonition >}}

## vim
```
sudo apt-get install vim
```
## SVN
```
sudo apt-get install rabbitvcs-cli
sudo apt-get install rabbitvcs-core
sudo apt-get install rabbitvcs-nautilus
```

## 分屏终端
```
sudo apt-get install terminator
```

## android studio
```
sudo snap install android-studio --classic
```

## 数据库查看软件
```
sudo atp-get install sqlitebrowser
```

## 媒体数据查看软件
```
sudo apt-get install mediainfo-gui
```

## 画图工具
```
sudo apt-get install pinta
```

## [搜狗输入法](https://shurufa.sogou.com/linux)
```
a) 执行命令安装 sudo dpkg -i sogoupinyin_2.4.0.3469_amd64.deb
b) 第一次安装会失败，失败后执行如下命令安装依赖，依赖安装后重新安装一次
		sudo apt-get install -f 
c) 在程序屋中找到Fcitx Configure 配置搜狗输入发
d）setting -> Region & Language -> 安装语言
e）将keyboard input method system 配置成fctix
f) 重启电脑
```
