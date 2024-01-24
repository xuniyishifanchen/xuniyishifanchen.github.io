---
title: "Vps 搭建脚本"
date: 2023-04-18T23:48:01+08:00
draft: false
tags: ["linux", "vpn"]
categories: ["vpn"]
password: 12138
---

## DD脚本
leitbogioro大佬的
<https://hostloc.com/thread-1159839-1-1.html>

<https://github.com/leitbogioro/Tools>
```
wget --no-check-certificate -qO InstallNET.sh 'https://raw.githubusercontent.com/leitbogioro/Tools/master/Linux_reinstall/InstallNET.sh' && chmod a+x InstallNET.sh
```
```
bash InstallNET.sh -debian 11 -pwd 'Vps.' -port "22"
```

萌咖大佬的《一键 DD Linux 系统脚本》
<https://meledee.com/2022/12/4112.html>

““需要删除
```
bash <(wget --no-check-certificate -qO- 'https://raw.githubusercontent.com/MoeClub/Note/master/InstallNET.sh') -d 11 -v 64 -p "密码" -port "端口"
```
eg:
```
bash <(wget --no-check-certificate -qO- 'https://raw.githubusercontent.com/MoeClub/Note/master/InstallNET.sh') -d 11 -v 64 -p 19950122Vps. -port 22
```

## bbr plus
<https://github.com/ylx2016/Linux-NetSpeed>
```
wget -O tcp.sh "https://github.com/ylx2016/Linux-NetSpeed/raw/master/tcp.sh" && chmod +x tcp.sh && ./tcp.sh
```
```
wget -N --no-check-certificate "https://github.000060000.xyz/tcpx.sh" && chmod +x tcpx.sh && ./tcpx.sh
```
<https://github.com/jinwyp/one_click_script>
```
bash <(curl -Lso- https://git.io/kernel.sh)
```

## 魔改x-ui
<https://github.com/MHSanaei/3x-ui>

<https://github.com/FranzKafkaYu/x-ui>
```
bash <(curl -Ls https://raw.githubusercontent.com/FranzKafkaYu/x-ui/master/install.sh)
```

## 三网回城测试
```
curl https://raw.githubusercontent.com/zhanghanyun/backtrace/main/install.sh -sSf | sh
```
```
wget -qO- git.io/autobesttrace | bash
```
```
wget -qO- git.io/besttrace | bash
```


## 解锁
### 流媒体
<https://github.com/lmc999/RegionRestrictionCheck>
```
bash <(curl -L -s check.unlock.media)
```
<https://github.com/LovelyHaochi/StreamUnlockTest>
```
bash <(curl -sSL "https://git.io/JswGm")
```

## 融合怪<https://github.com/spiritLHLS/ecs>
```
bash <(wget -qO- bash.spiritlhl.net/ecs)
```

## 更新环境
```
apt update -y  && apt upgrade -y && apt install -y curl wget sudo socat
```

## 当前主机ip
```
curl ifconfig.me/all
```

## Dokcer安装
```
curl ifconfig.me/all
```