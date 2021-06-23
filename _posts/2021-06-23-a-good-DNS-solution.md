---
layout: post
title:  "一个令人满意的DNS解决方案"
date:   2021-06-23 08:40:00 +0800
categories: blog
---

废话少说:

- 国内VPS上搭建AdGuardHome，开启添加ECS，图它的DoT、DoH服务、域名拦截、数据统计功能。

- 另在HK、JP或者SGP等境外较近的地域，用nginx或者其他软件搭一个SNI Proxy，转发dns.alidns.com到其目的地。

AdGuardHome的上游服务器设为https://dns.alidns.com，但是转发的目的地为上面部署的SNI Proxy监听的地址和端口。这样，图的是阿里云在香港的DNS服务，能满足延迟低、国内CDN友好、防污染的需求。

再多说一句，境外主流的DoH、DoT服务已被针对SNI进行阻断。

前面的区域，以后再来探索吧~