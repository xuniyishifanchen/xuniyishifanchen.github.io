---
title: "Docker - Pandora"
date: 2023-05-29T23:48:01+08:00
draft: false

lightgallery: true

math:
  enable: true

tags: ["Docker"]
categories: ["Docker"]
---

# Docker 搭建Pandora chatgpt
<https://github.com/pengzhile/pandora>

## 1 docker环境配置

```
wget -qO- get.docker.com | bash
```

## 2 安装运行pandora
```
docker pull pengzhile/pandora
docker run  -e PANDORA_CLOUD=cloud -e PANDORA_SERVER=0.0.0.0:8899 -p 8899:8899 -d pengzhile/pandora
```

## 3 获取token accessToken 字段
<https://chat.openai.com/api/auth/session>

## 4 访问本地链接
<http://127.0.0.1:8899>
