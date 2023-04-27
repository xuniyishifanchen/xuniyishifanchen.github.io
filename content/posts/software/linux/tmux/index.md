---
title: "Linux Software -  Tmux"
date: 2023-04-19T00:04:34+08:00
draft: false

lightgallery: true

math:
  enable: true

tags: ["software", "linux"]
categories: ["linux software"]
---

# Tmux
>ssh 后台多路复用，防止ssh断开连接之后程序退出。

{{<youtube -PUGYFlV0_g>}}
## 安装
```bash
sudo apt-get install tmux
```

## 使用
### 创建会话
```bash
tmux new -s my1
```
### 重新进入会话
```bash
tmux attach-session -t my1
```
### 查看所有tmux会话
```bash
tmux list-sessions
or 
tmux ls
```
### 删除会话
```bash
tmux kill-session -t my1
```
