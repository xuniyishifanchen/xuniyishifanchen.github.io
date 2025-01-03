---
title: "Linux Software - Oh my zsh"
date: 2023-04-19T00:03:08+08:00
draft: false

lightgallery: true

math:
  enable: true

tags: ["software", "linux"]
categories: ["linux software"]
---

>一款终端美化工具，详见：<https://www.jianshu.com/p/4a90675a6e6b>

## 安装zsh
```bash
sudo apt-get install zsh
```

## 切换到zsh
```bash
chsh -s /bin/zsh
``` 
## 安装oh my zsh
```bash
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

## [主题配置](https://www.jianshu.com/p/9c3439cc3bdb)
### powerlevel主题配置
```bash
将powerlevel9k.zip 解压后放到 ~/.oh-my-zsh/custom/themes
在~/.zshrc 配置 ZSH_THEME="powerlevel9k/powerlevel9k" 
```
[powerlevel9k.zip](/other/powerlevel9k.zip)

### 配置主题样式环境变量
```bash
POWERLEVEL9K_LEFT_PROMPT_ELEMENTS=( dir vcs)
POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=(status time)
POWERLEVEL9K_MODE='awesome-fontconfig'
```

### 字体安装
```bash
git clone https://github.com/powerline/fonts.git --depth=1
cd fonts
./install.sh
```
