---
layout: post
title:  "从Windows 10家庭中文版（OEM）升级到Windows 10专业版"
date:   2020-09-05 23:00:32 +0800
categories: blog
---

*没想到，竟然折腾了一下午*

- 缘由

看到v2ex上一帖[Win+ Linux 双系统到底有什么弊端和优点？](https://www.v2ex.com/t/704231)，感觉linux GUI实在没使用的必要，遂放弃win+arch双系统，将使用linux的需求转移到[ArchWSL2](https://github.com/yuk7/ArchWSL2)。然后心痒，想折腾一下Windows系统，因为hyper-v虚拟机和Windows sandbox的特性，决心升级到Windows 10 Pro。

- 准备

从[官网](https://docs.microsoft.com/en-us/windows-server/get-started/kmsclientkeys)拿到kms key，虚拟机自建kms服务器。

- 过程

直接输入key报无效，各种key都是，错误代码**0xC004F0690**，遂发动查资料技能，一开始没找到解决方案。怀疑是设想错误，不该这么装。在中文搜索结果和淘宝key各种轰炸之后，对MAK、Retail和OEM等有了初步认知，但却更加也使我相信自己一开始的设想没错：应该就是**输入官方key后直接用kms激活**。

- 转机

最重要的转机是[这里](https://gist.github.com/D4rk4/2fc3355c39a1325641df50141f38843c)，提供了如下的命令：

```markdown
DISM.exe /Online /Get-TargetEditions
```

然后就简单了，管理员状态cmd直接输入，报错**0x800f0805**。

继续搜索，从[这里](https://www.howtoedge.com/fix-update-error-0x800f0805-in-windows-10/)和[这里](http://jiaocheng.386w.com/jiaocheng/win10_article_57671.html)，拿到如下命令：

```markdown
Dism /Online /Cleanup-Image /ScanHealth
DISM /Online /Cleanup-image /RestoreHealth
//reboot
sfc /SCANNOW
```

怀疑是使用过程中某些系统文件发生了变化，这里是进行了恢复。

然后重新输入上述命令，顺利显示出可以升级的TargetEditions。

设置里输入Windows Pro的kms key，成功。

- 总结

折腾是第一生产力。善用搜索引擎，但前提是，先动动脑子。

ps:过程中多次删除了自带的key，多亏[这个问答](https://answers.microsoft.com/zh-hans/windows/forum/all/win10%E5%AE%B6%E5%BA%AD%E7%89%88%E5%8D%B8%E8%BD%BD/54a59062-322b-457a-8b08-862848f58663)，拿到了原本的key并进行备份。PowerShell代码放这里：

```markdown
(Get-WmiObject -query 'select * from SoftwareLicensingService').OA3xOriginalProductKey
```

