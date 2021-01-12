---
layout: post
title:  "用户侧DNS折腾小计"  
date:   2020-09-06 14:41:32 +0800
categories: blog
---

*前前后后折腾了两三个月了，最后发现，平平淡淡才是真*  

## 缘起  

好像自己有点网络洁癖。年初从Android 9+系统自带的Private DNS了解到DoT，就一直念念不忘。主要集中在解决下面三个问题上面：  
- 一是寻找更好的DNS公共服务器  
- 二是解决跨平台使用的问题（Android，Windows 10，Linux）  
- 三是平衡功能，易用性，延迟和准确度四者之间的关系  

## 缘中  

第一，即了解各种DNS相关的协议，主要是DNSSEC，DNSCrypt，DNS-over-tls，DNS-over-https  
DNSSEC，了解不多，貌似只加密（保护隐私），不防篡改，与后面的二者兼顾相比，好像用处不大  
DNSCrypt，挺好，但没有发展为国际标准  
DoT，DoH已有RFC标准，发展比较成熟  
（最近了解到还有DNS-over-quic，有点意思）  

然后就是公共DNS服务器，我个人的要求，大厂出品，国内的看延迟，国外的看延迟和对国内CDN的友好程度（准确度）。  
比较全的列表：[来自DNSCrypt](https://dnscrypt.info/public-servers)，[来自cURL](https://github.com/curl/curl/wiki/DNS-over-HTTPS#publicly-available-servers)  
简单总结：  
国内：alidns，dnspod都支持DoH，DoT，但前者老抽风，起码我的两个域名已经连续一两个月不能解析了，后者还处于测试阶段。360 DNS支持DoH。rubyfish可用，但不是我的首选。  
国外：cloudflare出于隐私考虑，不支持ECS，解析国内域名十分难受，基本没咋用。Google DoT总体效果还行，DoH被墙。Nextdns可以自定义，功能多，免费用户30万次/mo，电信访问的韩国入口，延迟在60-70ms，但对移动不友好。Opendns有国内节点，无污染，延迟还行，但无DoT。  

第二，客户端问题，Android自带DoT不用多考虑，主要是Windows 10和Linux，unbound支持DoT，DNSCrypt-proxy支持三样，还有就是AdGuardHome，支持很全，有web管理界面，三者在Windows 10上均可部署为服务开机自启，好评。  
另外的小工具像mos-chinadns也好评，配置很丰富。  

第三，我的方案  
电脑端先后经历过：单纯unbound或DNSCrypt-proxy----AdGuardHome+去广告规则----AdGuardHome+自带分流+去广告规则（这里要特别提到[GFWList2AGH](https://github.com/hezhijie0327/GFWList2AGH)这个项目，十分感谢）----单纯unbound，alidns，省心（因为去广告和获取无污染DNS根本不需要DNS客户端来做，浏览器插件和v2ray已经做了）  
而Android最优选则是自定义，国内vps上部署AdGuardHome DoT服务，去广告+分流，满足以上4项要求，不能再省心了。（这里需要特别提到的是，之所以需要分流，是因为我发现v2ray手机客户端无法实现远端解析，被系统DoT服务“拦截”掉了，所以必须先获取无污染的DNS解析结果）  

## 缘落  

大致告一段落了，以后再想到什么再来更新(>_<)  
