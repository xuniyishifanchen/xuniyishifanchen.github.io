---
title: "Docker - Typecho"
date: 2023-05-29T23:48:01+08:00
draft: true

lightgallery: true

math:
  enable: true

tags: ["Docker"]
categories: ["Docker"]
---

# Docker 搭建Typecho

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

networks:
  typecho:
    external: false

services:
  server:
    image: joyqi/typecho:nightly-php7.3-apache
    container_name: typecho
    environment:
     - TIMEZONE=Asia/Shanghai
     - TYPECHO_DB_ADAPTER=Pdo_Pgsql
     - TYPECHO_DB_HOST=db
     - TYPECHO_DB_PORT=5432
     - TYPECHO_DB_USER=typecho
     - TYPECHO_DB_PASSWORD=typecho
     - TYPECHO_DB_DATABASE=typecho
     - TYPECHO_SITE_URL=http://10.0.0.200
    restart: always
    networks:
      - typecho
    volumes:
      - ./typecho:/app/usr
    
    ports:
      - "80:80"
      - "222:22"
    depends_on:
      - db

  db:
    image: postgres:14
    restart: always
    environment:
      - POSTGRES_USER=typecho
      - POSTGRES_PASSWORD=typecho
      - POSTGRES_DB=typecho
    networks:
      - typecho
    volumes:
      - ./postgres:/var/lib/postgresql/data
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
