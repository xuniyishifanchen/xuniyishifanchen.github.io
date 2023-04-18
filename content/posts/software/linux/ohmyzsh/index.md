---
title: "Ohmyzsh"
date: 2023-04-19T00:03:08+08:00
draft: false

lightgallery: true

math:
  enable: true

tags: ["software", "linux"]
categories: ["linux software"]
---

# linux 终端美化

详见：
>https://www.jianshu.com/p/4a90675a6e6b

## 安装zsh
```
sudo apt-get install zsh
```
## 安装oh my zsh
```
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```
## 切换到zsh
```
chsh -s /bin/zsh
``` 

## [主题配置](https://www.jianshu.com/p/9c3439cc3bdb)
### powerlevel主题配置
```
将powerlevel9k.zip 解压后放到 ~/.oh-my-zsh/custom/themes
在~/.zshrc 配置 ZSH_THEME="powerlevel9k/powerlevel9k" 
```
[powerlevel9k.zip](../../assets/powerlevel9k.zip)

### 配置主题样式环境变量
```
POWERLEVEL9K_LEFT_PROMPT_ELEMENTS=(dir vcs)
POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=(status time)
```

### 字体安装
```
git clone https://github.com/powerline/fonts.git --depth=1
cd fonts
./install.sh
```