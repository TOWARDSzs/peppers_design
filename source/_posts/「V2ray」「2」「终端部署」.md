---
title: 「V2ray」「2」「终端部署」
date: 2020-02-13 22:58:09
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

通过一键脚本部署v2ray服务，之后，参考本篇博文配置终端

本文禁止任何形式的转载
***
*  你已经在国外的server上配置成功了v2ray服务
 *  我的建议是http伪装
 *  此时你拥有了服务器配置的Vmess地址
*   你需要了解的        
 *  请尽量使用PAC mode，一般不轻易使用global mode
  * 注意：
      1. 【推荐】一般来说单独使用V2ray PAC mode就足够了，如遇规则无法判断的网址，更改PAC配置文件即可
      2. 【不推荐新手】把V2ray客户端设置成手动模式，并搭配Proxifier等软件或者Chrome SwitchOmega插件使用。

## Windows
  客户端： V2RayN    
  测试版本： 2.41
  * 配置服务器 【Vmess配置连接】-> [服务器选项卡] -> 批量导入URL即可添加
  * 开启服务： 选择服务器，开启代理，测试google
  * 设置开机启动：参数设置选项卡
  ![图片信息](/img/V2.png)
  fig.1
  ![图片信息](/img/V2ray.png)
  fig.2

##  MAC OS
  ![图片信息](/img/V.png)
  https://github.com/Cenmrev/V2RayX/releases


## iphone/ipad
  美国区apple store下载kitsunebi。账号就不分享了，各位自己想办法。
