---
categories: 问题随笔
tags:
  - v6-proxy
  - chatgpt
description: 使用TunnelBroker提供的免费/48的ipv6解决chat-to-api的429问题
permalink: chat-to-api-solving-429/
title: 使用TunnelBroker提供的免费/48的ipv6解决chat-to-api的429问题
cover: /images/db9e969544fc528abc752fa8dfd84c3b.jpg
date: '2024-05-03 15:51:00'
updated: '2024-05-24 13:38:00'
---

**5月24日更新**


**TunnelBroker的ip应该已经被openai禁止免登录，所以本文可能达不到你想要的目的。**


原文


最近免登录的chat to api会频繁出现429，本文教你如何解决这个问题。


## 配置隧道


首先你要有一个拥有公网IPv4，且防火墙允许ICMP、TCP、6in4协议通过的VPS，大部分的系统应该都是可以的，下文以`ubuntu 22.04`为例。

注册一个TunnelBroker的账号 https://tunnelbroker.net/

注册成功之后创建一个隧道。（注意新号注册当天无法申请/48，可能需要等一天）

成功创建隧道之后，找到`Example Configurations`，找到你的系统的配置样例，配置到你的系统上，因为我是用的是`ubuntu 22.04`，这个系统当前使用的是`netplan` 管理网络，因此我选择的是`netplan`


![Untitled.png](/images/372c39cdb851bef9ac8076fd8a0be887.png)


配置好了之后，你的服务器因该就有了一个ipv6地址了。可以通过这个命令来进行测试


```bash
curl ipv6.ip.sb
```


从你的子网中随机找一个ip 比如 2001:0470:0007:0082:6a4b:2c8f:1d3e:5f9a (不知道怎么找可以把网段给chatgpt让他找)


```javascript
sysctl -w net.ipv6.ip_nonlocal_bind=1
sysctl -w net.ipv6.conf.all.forwarding=1

curl --int 2001:0470:0007:0082:6a4b:2c8f:1d3e:5f9a ipv6.ip.sb
```


如果这里成功返回你的地址证明你配置ok


如果这里不通，说明你配置的有问题，请回去检查配置。也有可能是服务商不支持，注意一下local可能是你的公网ip，也可能是内网ip，和服务商配置有关，一般都是公网ip，NAT是内网ip。



这样你就至少有了一个/64的ipv6地址了，等一天之后你可以点一下申请/48的地址。


## 启动代理服务


有了地址之后，我们需要一个程序把这些地址变成一个标准的代理服务，方便chat to api使用。可以使用我刚写的这个，可以把ipv6地址段变成一个随机的代理池，使用方法我都放在readme中了。其中的`--cidr`就是在上述文章中申请到的`/64` 或者`/48`的地址段。


[https://github.com/zbronya/v6-proxy](https://github.com/zbronya/v6-proxy)


## 其他说明

- 不限于chatgpt，只要是有ipv6解析的有ip限制的服务都是可以用的
- 代理程序是通用的，如果不喜欢TunnelBroker，或者你有其他的`/32` `/29`的地址，也是可以使用的
- 代理程序我只在`ubuntu 22.04`的amd和arm系统上测试过，其他的系统如果有问题可以提issue或者pr
- 帮我程序点个star吧
- 如果你还没有chat-to-api的项目，也可以试试我的  [https://github.com/zbronya/free-chat-to-api](https://github.com/zbronya/free-chat-to-api)
