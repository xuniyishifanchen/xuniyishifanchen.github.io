---
title: "Linux Software - Git"
date: 2023-04-19T00:08:58+08:00
draft: false

lightgallery: true

math:
  enable: true

tags: ["linux", "cmd"]
categories: ["linux cmd"]
---

# git 命令

## 查看当前状态
```
git status
```
## 添加修改的文件到暂存区
```
git add .
```
## 将暂存区的文件提交
```
git commit
```
## 在上一次提交的基础上修改提交
```
git commit --amend
```
## 在上一次提交的基础上修改提交提交者
```
git commit --amend --author="xxxx@gmail.com"
```
## 查看当前branch
```
git branch
```
## 查看所有branch
```
git branch -a
```
## 查看当前branch 详细信息
```
git branch -vv
```
## 将现有修改cherry-pick 下来，并携带cherry-pick 信息
```
git cherry-pick -ex
```
## 下载指定分支的代码
```
git clone ssh:xxxxx -b branch
```
## 向指定分支提交代码
```
git push ssh:xxx HEAD:refs/for/master
```
## 将开发分支merrge到主分支
```
git merge --no-ff -m "Merge remote-tracking branch 'origin/master' into feature" origin/master
```

## 查看当前最近的TAG
```
git describe --match \"[0-9]*.[0-9]*.[AZ]*.[0-9]*.[0-9]*\" --abbrev=0
```
## 修改切换账户后不能上传问题
```
git config --global --unset credential.helper
```