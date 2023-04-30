---
title: "Linux Software - Adb"
date: 2023-04-19T00:08:54+08:00
draft: false

lightgallery: true

math:
  enable: true

tags: ["linux", "cmd"]
categories: ["linux cmd"]
---

# adb 命令集合

## 安装应用
```
adb install -d -r -t xxxx.apk
-d: 降版本安装
-r: 替换安装
-t: 允许安装测试apk
```

## 卸载应用
```
adb uninstall package_name
or
adb shell pm uninstall packagename
```

## 解锁手机
```
adb root
adb disable-verity
adb reboot
adb remount
```

## 发送低电广播
```
adb shell am broadcast -a android.intent.action.BATTERY_CHANGED --ei level 5
```

## 跳过开机导航
```
adb shell settings put secure user_setup_complete 1
adb shell settings put global device_provisioned 1
```

## 设置log的等级
```
adb shell setprop log.tag.XXXX VERBOSE
```

## 获取当前显示的Activity
```
adb shell  dumpsys activity | grep mResumedActivity
```

## 拉取安装的apk
```
adb shell  dumpsys activity | grep mResumedActivity
adb shell pm list packages
adb shell pm path "packageName"
adb pull "apk path"
```

## 查看jobscheduler信息
```
adb shell dumpsys jobscheduler
```

## 查看手机屏幕信息
```
adb shell dumpsys window displays
```

## 查看相机信息
<https://source.android.com/docs/core/camera/debugging?hl=zh-cn>
```
adb shell dumpsys media.camera
```

## 实时查看相机信息
```
watch -n 0.01 -d -t "adb shell dumpsys media.camera | egrep -A1 targetBurstFrameRate"
```

## 查询图片信息
```
adb shell content query --uri ccontent://media/external/images/media
```

## 模拟tap事件
```
adb shell input tap 402 2077
```

## 模拟按键事件
```
adb shell input keyevent 4
```
## 模拟文字输入
```
adb shell input text "xxx"
```