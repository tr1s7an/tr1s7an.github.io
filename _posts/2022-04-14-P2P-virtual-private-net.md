---
layout: post
title:  "P2P VPN 解决方案大赏"
date:   2022-04-14 14:10:32 +0800
categories: blog
---

### zerotier

- 最成熟的商业化方案，IPv4 && IPv6，加密udp传输，打洞效果优秀

- 中心化的planet用于下发配置，提供moon辅助打洞（planet和moon均可自建，就是比较麻烦），没有数据中转

- 全平台可用

- 比较膈应的是，作为tier-2的VPN，其windows端不得不采用了low到爆的tap虚拟网卡

### tailscale

- 新的，比较成熟的商业化方案，IPv4 && IPv6，加密udp传输（基于安全高效的wireguard）

- 中心化的服务器提供公钥交换服务（可自建headscale中心服务器），提供derp节点用于数据中转（tcp+tls，也可自建）

- 全平台可用，但无法直接使用wireguard客户端连接

### netmaker

- 新的，不太成熟的开源方案，与tailscale类似，基于wireguard

- netmaker服务端需要自建，作为中心服务器掌控网络配置（目前只支持linux）

- netclient客户端全桌面端可用，但可以使用wireguard客户端作为外部客户端接入网络，四舍五入全平台可用（netclient会自动设置进程守护，不得不说很方便）

- netmaker-ui看起来很难受，使用不友好

- 文档推崇docker方式部署，直接部署的文档不清晰，安装脚本也有点问题

- 目前bug不少：尝试开启双栈后，netclient只创建IPv6地址；netclient不会自动pull config，从ui上看，5分钟状态warning，30分钟状态error。。。

- 看好它的前景

### nebula

- 比较新的，比较成熟的开源方案，不支持IPv6，加密udp传输

- 与前面三者不同的是，无中心化的服务器，即网络加入新节点需要改动所有端的配置

- 全平台可用

- 目前开发似乎处于停滞期


### 其他： n2n, tinc等

- 看起来没啥吸引人的地方，没有尝试的兴趣
