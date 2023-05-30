---
title: "Linux Software - Docker"
date: 2023-04-19T00:08:54+08:00
draft: false

lightgallery: true

math:
  enable: true

tags: ["linux", "cmd"]
categories: ["linux cmd"]
---

# Docker
>可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。

## 1 安装docker

```
wget -qO- get.docker.com | bash
```

## 2 安装docker-compose
```
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose --version
```

## 3 设置docker 开机启动
```
systemctl enable docker
```

## 4 查看docker版本
```
docker -v
```
## 5 卸载docker
```
sudo apt-get purge docker-ce docker-ce-cli containerd.io
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```
## 6 修改docker 配置
```
cat > /etc/docker/daemon.json <<EOF
{
    "log-driver": "json-file",
    "log-opts": {
        "max-size": "20m",
        "max-file": "3"
    },
    "ipv6": true,
    "fixed-cidr-v6": "fd00:dead:beef:c0::/80",
    "experimental":true,
    "ip6tables":true
}
EOF
```
## 7 重启docker服务
```
systemctl restart docker
```





