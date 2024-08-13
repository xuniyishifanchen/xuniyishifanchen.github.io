---
title: "Linux Software - Gerrit"
date: 2024-08-08T00:08:58+08:00
draft: false

lightgallery: true

math:
  enable: true

tags: ["software", "cmd"]
categories: ["linux software"]
---

## Gerrit 搭建流程

### 1 安装java 17
```
sudo apt install openjdk-17-jdk
```

### 2 安装git
```
sudo apt-get install git
```

### 3 创建目录并下载gerrit 安装包
[](https://www.gerritcodereview.com/)
```
mkdir ~/gerrit_site
cd ~/gerrit_site
wget https://gerrit-releases.storage.googleapis.com/gerrit-3.10.1.war
```

### 4 安装gerrit
```
java -jar gerrit-3.10.1.war init --batch -d .
```

### 5 配置插件
```
unzip -oq gerrit-3.10.1.war -d gerrit-3.10.1
cp -r gerrit-3.10.1/WEB-INF/plugins/ plugins/
```
### 6 配置gerrit
将Auth方式从OPENID改成HTTP
```
vim etc/gerrit.config
```

```
[gerrit]
        basePath = git
        canonicalWebUrl = http://jppro2-202311062247356c80ec.debian:8080/
        serverId = a77a650a-401d-4168-94c1-5b3c641873b3
[container]
        javaOptions = "-Dflogger.backend_factory=com.google.common.flogger.backend.log4j.Log4jBackendFactory#getInstance"
        javaOptions = "-Dflogger.logging_context=com.google.gerrit.server.logging.LoggingContext#getInstance"
        user = root
        javaHome = /usr/lib/jvm/java-17-openjdk-amd64
[index]
        type = lucene
[auth]
        type = HTTP
[receive]
        enableSignedPush = false
[sendemail]
        smtpServer = localhost
[sshd]
        listenAddress = *:29418
[httpd]
        listenUrl = http://*:8080/
[cache]
        directory = cache
~                             
```

### 7 配置apache2 反向代理
```
sudo apt-get install apache2
sudo a2enmod proxy
sudo a2enmod proxy_http
vim /etc/apache2/apache2.conf
```

添加如下配置
```
Listen 8090
<VirtualHost *:8090>
    ServerName gerrit
    ProxyRequests Off
    ProxyVia Off
    ProxyPreserveHost On
    <Proxy *>
          Order deny,allow
          Allow from all
    </Proxy>
    <Location /login/>
        AuthType Basic
        AuthName "Gerrit Code Review"
        AuthBasicProvider file
        AuthUserFile "~/gerrit/etc/passwords"
        Require valid-user
    </Location>
    AllowEncodedSlashes On
    ProxyPass / http://127.0.0.1:8080/
</VirtualHost>

```
重启服务查看状态
```
sudo /etc/init.d/apache2 restart
sudo systemctl status apache2.service
```

### 8 创建用户
```
htpasswd -c ~/gerrit/etc/passwords admin
```

### 9 添加用户
```
htpasswd -m ~/gerrit/etc/passwords person1
```
### 10 启动gerrit 服务
```
./bin/gerrit.sh start
```

### 11 访问服务ip：8090


遇到的问题：
```
1. passwords 文件apache2 无法访问，需要给目录和文件都加权限。
调试可以查看/var/log/apache2/error.log
```


