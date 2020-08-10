---
title: 「V2ray」「1」「基本部署」
date: 2020-02-13 22:39:36
tags:
- V2ray
- Web
categories: V2ray
copyright:
---
<meta name="referrer" content="no-referrer"/>

***
写在前面：



v2ray的部分，在这里记录得很含糊，是为了自己参考使用，而不是用来分享，当然大家可以参考，请用作正当用途，不当使用导致的后果，博主不予承担

已经生成了常见组合和配置，如果这里没有你想要的配置，简单的方法是试试这个程序的[在线版本](https://veekxt.com/utils/v2ray_gen).

本文禁止任何形式的转载
***
[V2Ray一键安装脚本·233boy
/
v2ray](https://github.com/233boy/v2ray/wiki/V2Ray%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%85%E8%84%9A%E6%9C%AC)
方案：
http2
port: 54321
/router
centos7

### Firewall config
```
# 启动防火墙并且永久添加当前ssh端口
systemctl start firewalld && firewall-cmd --zone=public --add-port=26088/tcp --permanent
systemctl start firewalld && firewall-cmd --zone=public --add-port=22/tcp --permanent
# 开放必要的端口
firewall-cmd --zone=public --add-port=80/tcp --permanent
firewall-cmd --zone=public --add-port=443/tcp --permanent
firewall-cmd --zone=public --add-port=54321/tcp --permanent
firewall-cmd --reload
# 查看当前开放端口
systemctl status firewalld
# 开机启动
systemctl enable firewalld

# 其他命令参考：
# 启动：
systemctl start firewalld
# 关闭：
systemctl stop firewalld
# 查看状态：
systemctl status firewalld
# 开机禁用  ：
systemctl disable firewalld
# 开机启用  ：
systemctl enable firewalld
# 查看所有打开的端口：
firewall-cmd --zone=public --list-ports
# 那怎么开启一个端口呢
# 添加
firewall-cmd --zone=public --add-port=80/tcp --permanent #（--permanent永久生效，没有此参数重启后失效）
# 重新载入
firewall-cmd --reload
# 查看端口号是否开启，运行命令：
firewall-cmd --zone=public --query-port=80/tcp
# 删除
firewall-cmd --zone=public --remove-port=80/tcp --permanent
```
