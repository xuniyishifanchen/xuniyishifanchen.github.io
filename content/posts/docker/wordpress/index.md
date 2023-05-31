---
title: "Docker - Wordpress"
date: 2023-05-29T23:48:01+08:00
draft: false

lightgallery: true

math:
  enable: true

tags: ["Docker"]
categories: ["Docker"]
---

# Docker 搭建Wordpress

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
version: "3.9"
services:
  db:
    image: mysql:5.7
    volumes:
      - ./db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    volumes:
      - ./wordpress_data:/var/www/html
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
```

## 3 启动
```
后台启动：docker-compose up -d
前台启动：docker-compose up
停止：docker-compose down
重启：docker-compose restart
```

## 4 访问
<http://IP:8000>
