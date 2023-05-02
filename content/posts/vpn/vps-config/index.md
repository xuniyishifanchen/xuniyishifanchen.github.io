---
title: "Vps 环境搭建"
date: 2023-04-18T23:48:01+08:00
draft: false
tags: ["linux", "vpn"]
categories: ["vpn"]
password: 12138
---
{{<youtube s90feRmdr9A>}}
## 1 启用root账户并开启密码登陆：

### 切换到root用户：
```
sudo -i
```
### 编辑配置文件：
```
vim /etc/ssh/sshd_config
修改参数：PermitRootLogin
修改参数：PasswordAuthentication
```
### 修改密码：
```
passwd
```
### 重启ssh服务：
```
systemctl restart ssh
```

## 2 购买域名: <https://www.namesilo.com/>

## 3 升级linux bbr plus内核：
>https://github.com/jinwyp/one_click_script
```bash
bash <(curl -Lso- https://git.io/kernel.sh)
```

## 4 节点搭建：
### 更新软件源
```bash	
apt update
```
### 安装x-ui，并切换xray内核到最新版本：
```bash
bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh)
```
### 安装nginx
```bash
apt install nginx
```
### 安装acme：
```bash
curl https://get.acme.sh | sh
```
### 添加软链接：
```bash
ln -s  /root/.acme.sh/acme.sh /usr/local/bin/acme.sh
```
### 切换CA机构：
```bash
acme.sh --set-default-ca --server letsencrypt
```
### 申请证书：
```bash
acme.sh  --issue -d 你的域名 -k ec-256 --webroot  /var/www/html
```
### 安装证书：
```bash
acme.sh --install-cert -d 你的域名 --ecc --key-file       /etc/x-ui/server.key  --fullchain-file /etc/x-ui/server.crt --reloadcmd     "systemctl force-reload nginx"
```

### 配置 nginx
```bash
vim /etc/nginx/nginx.conf
```
```json
user www-data;
worker_processes auto;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
    worker_connections 1024;
}

http {
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_timeout 65;
    types_hash_max_size 2048;

    include /etc/nginx/mime.types;
    default_type application/octet-stream;
    gzip on;

    server {
        listen 443 ssl;
        
        server_name healer.cloud;  #你的域名
        ssl_certificate       /etc/x-ui/server.crt;  #证书位置
        ssl_certificate_key   /etc/x-ui/server.key; #私钥位置
        
        ssl_session_timeout 1d;
        ssl_session_cache shared:MozSSL:10m;
        ssl_session_tickets off;
        ssl_protocols    TLSv1.2 TLSv1.3;
        ssl_prefer_server_ciphers off;

                location / {
            proxy_pass https://www.fan-2000.com/; #伪装网址
            proxy_redirect off;
            proxy_ssl_server_name on;
            sub_filter_once off;
            sub_filter "www.fan-2000.com" $server_name;
            proxy_set_header Host "www.fan-2000.com";
            proxy_set_header Referer $http_referer;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header User-Agent $http_user_agent;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto https;
            proxy_set_header Accept-Encoding "";
            proxy_set_header Accept-Language "zh-CN";
        }


        location /6cd643ae-e71f-40ca-9803-b45109f4bade {   #分流路径
            proxy_redirect off;
            proxy_pass http://127.0.0.1:10000; #Xray端口
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
        
        location /6cd643ae-e71f-40ca-9803-b45109f4bade-xui {   #xui路径
            proxy_redirect off;
            proxy_pass http://127.0.0.1:9999;  #xui监听端口
            proxy_http_version 1.1;
            proxy_set_header Host $host;
        }
    }
    	
   server {
        listen 80;
        location /.well-known/ {
               root /var/www/html;
            }
        location / {
                rewrite ^(.*)$ https://$host$1 permanent;
            }
    }
}
```
### 重新加载nginx:
```bash
systemctl reload nginx
```

## 5 netflix 解锁
### 下载检测解锁netflix 程序：
```bash
wget -O nf https://github.com/sjlleo/netflix-verify/releases/download/v3.1.0/nf_linux_amd64 && chmod +x nf
```
### 检测解锁情况
```bash
./nf
```
### 通过代理检查解锁情况
```bash
./nf -proxy socks5://127.0.0.1:40000
```

## 6 使用wrap代理
>官方教程：<https://pkg.cloudflareclient.com/install>

### 安装WARP仓库GPG 密钥：
```bash
curl https://pkg.cloudflareclient.com/pubkey.gpg | sudo gpg --yes --dearmor --output /usr/share/keyrings/cloudflare-warp-archive-keyring.gpg
```
### 添加WARP源：
```bash
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/cloudflare-warp-archive-keyring.gpg] https://pkg.cloudflareclient.com/ $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/cloudflare-client.list
```
### 更新APT缓存：
```bash
apt update
```
### 安装WARP：
```bash
apt install cloudflare-warp
```
### 注册WARP：
```bash
warp-cli register
```
### 设置为代理模式（一定要先设置）：
```bash
warp-cli set-mode proxy
```
### 连接WARP：
```bash
warp-cli connect
```
### 查询代理后的IP地址：
>wrap 默认代理40000端口
```bash
curl ifconfig.me --proxy socks5://127.0.0.1:40000
```

### 修改xui的xray配置并重启面板，让某些网站走wrap代理
```
{
  "api": {
    "services": [
      "HandlerService",
      "LoggerService",
      "StatsService"
    ],
    "tag": "api"
  },
  "inbounds": [
    {
      "listen": "127.0.0.1",
      "port": 62789,
      "protocol": "dokodemo-door",
      "settings": {
        "address": "127.0.0.1"
      },
      "tag": "api"
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    },
    {
      "tag": "netflix_proxy",
      "protocol": "socks",
      "settings": {
        "servers": [
          {
            "address": "127.0.0.1",
            "port": 40000
          }
        ]
      }
    },
    {
      "protocol": "blackhole",
      "settings": {},
      "tag": "blocked"
    }
  ],
  "policy": {
    "system": {
      "statsInboundDownlink": true,
      "statsInboundUplink": true
    }
  },
  "routing": {
    "rules": [
    {
        "type": "field",
        "outboundTag": "netflix_proxy",
        "domain": [
          "geosite:netflix",
          "geosite:disney",
          "geosite:openai"
        ]
      },
      {
        "inboundTag": [
          "api"
        ],
        "outboundTag": "api",
        "type": "field"
      },
      {
        "ip": [
          "geoip:private"
        ],
        "outboundTag": "blocked",
        "type": "field"
      },
      {
        "outboundTag": "blocked",
        "protocol": [
          "bittorrent"
        ],
        "type": "field"
      }
    ]
  },
  "stats": {}
}
```