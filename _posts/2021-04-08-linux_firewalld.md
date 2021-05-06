---
title: linux firewall
date: 2021-04-08 14:21:00
categories:
 - PRODUCT
tag:
 - RHEL
---

### 리눅스 방화벽 특정 IP 모든 포트 오픈

#### Whitelist a specific IP which grants access to any port

OS : RHEL 7.x, CentOS7.x

```bash
$ firewalld-cmd --permanant --zone=public --add-rich-rule='rule family="ipv4" source address="xx.xx.xx" accept'
```

<!-- more -->