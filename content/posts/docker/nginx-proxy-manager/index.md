---
title: "Docker - Nginx Proxy Manager"
date: 2023-05-29T23:48:01+08:00
draft: false

lightgallery: true

math:
  enable: true

tags: ["Docker"]
categories: ["Docker"]
---

# Docker 搭建Nginx Proxy Manager

## 1 预先docker环境配置

```
wget -qO- get.docker.com | bash
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

## 2 编写配置文件
```
vim docker-compose.yml
```
```
version: "3"

services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      # These ports are in format <host-port>:<container-port>
      - '80:80' # Public HTTP Port
      - '443:443' # Public HTTPS Port
      - '81:81' # Admin Web Port
      # Add any other Stream port you want to expose
      # - '21:21' # FTP
    environment:
      DB_MYSQL_HOST: "db"
      DB_MYSQL_PORT: 3306
      DB_MYSQL_USER: "npm"
      DB_MYSQL_PASSWORD: "npm"
      DB_MYSQL_NAME: "npm"
      # Uncomment this if IPv6 is not enabled on your host
      # DISABLE_IPV6: 'true'
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
    depends_on:
      - db

  db:
    image: 'jc21/mariadb-aria:latest'
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: 'npm'
      MYSQL_DATABASE: 'npm'
      MYSQL_USER: 'npm'
      MYSQL_PASSWORD: 'npm'
    volumes:
      - ./data/mysql:/var/lib/mysql
```

## 3 启动
```
后台启动：docker-compose up -d
前台启动：docker-compose up
停止：docker-compose down
重启：docker-compose restart
```

## 4 访问
<http://IP:81>

## 5 默认用户名密码
>Email:    admin@example.com

>Password: changeme
