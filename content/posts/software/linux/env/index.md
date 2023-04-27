---
title: "Other"
date: 2023-04-19T00:09:25+08:00
draft: false

lightgallery: true

math:
  enable: true

tags: ["linux", "cmd"]
categories: ["linux cmd"]
---

## 更新apt 源
> sudo apt-get update

## apt 阿里源
```
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```

## 解决软件依赖问题
```
apt --fix-broken install
```

## 生成一个70G大文件
```
dd if=/dev/zero of=bigFile1 bs=1M count=700
```

## 使用axel 下载东西
```
axel -aN6 -n 50 url
```

## 查找并替换字符串

```
find . -name "*.java" | xargs sed -i 's/old/new/g'
```

## 环境变量配置
```
export REPO_URL=/home/xc/App/repo/git-repo
export PATH=$PATH:XXX
export ANDROID_HOME=/home/xc/Android/Sdk
export JAVA_TOOL_OPTIONS='-Djava.net.preferIPv6Addresses=true'
export JAVA_OPTS='-Djava.net.preferIPv6Addresses=true'
```

## 自定义快捷指令
```
alias gD="update_copyright.sh;echo ./gradlew  assembleDebug spotBugs;./gradlew  assembleDebug;echo adb install -d -r -t app/build/outputs/apk/debug/*-xxhdpi-debug.apk;adb install -d -r -t app/build/outputs/apk/debug/*-xxhdpi-debug.apk"
alias gC="echo ./gradlew clean; ./gradlew  clean"
alias logg="adb logcat -c;adb logcat | grep"
alias lc="adb logcat -c"
alias ld="adb logcat | grep NEU" 
alias gv="git describe --match \"[0-9]*.[0-9]*.[AZ]*.[0-9]*.[0-9]*\" --abbrev=0"
alias dc="adb shell dumpsys media.camera > dumpsys.txt"
alias dcd="adb shell dumpsys media.camera | grep -E \"Device 0|Device 1|Device 2|Device 3|Device 4|Device 5\"" 
```

## java 环境配置
### java 8
```
sudo apt-get install openjdk-8-jdk
```
### java 11
```
sudo apt-get install openjdk-11-jdk
```
### java 环境切换
```
sudo update-alternatives --config java
```
### 配置JAVA 使用ipv6：
```
export JAVA_TOOL_OPTIONS='-Djava.net.preferIPv6Addresses=true'
export JAVA_OPTS='-Djava.net.preferIPv6Addresses=true'
```


