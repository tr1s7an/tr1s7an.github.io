---
layout: post
title:  "fly.io容器托管使用"
date:   2021-06-27 13:30:00 +0800
categories: blog
---

## 为何选择[fly.io](https://fly.io "fly.io")

作为白嫖党，遇到很对自己口味的免费福利，当然要毫不犹豫地下手了。fly.io主要提供docker容器托管服务，每月有充足的免费额度，但是注册需要验证信用卡（无扣款验证）。

可部署的区域如下：

[![https://fly.io/docs/reference/regions/](/assets/pics/flydotio-regions.png)](https://fly.io/docs/reference/regions/ "fly.io regions")

免费层级的额度如下：

[![https://fly.io/docs/about/pricing/](/assets/pics/flydotio-free-tier.png)](https://fly.io/docs/about/pricing/ "fly.io pricing")

简单说：

- 容器：每月2340小时，可供3个配置为shared-cpu-1x，256MB RAM的容器不间断运行。（3\*24\*31=2232<2340）

- 流量：只计流出量，各个地区的免费额度分别为：北美洲和欧洲共100GB，非洲、亚太地区（不包括印度）、大洋洲、南美洲共30GB，印度单独30GB，共计160GB。超出后不同地区计费也不一样，按照前述顺序分别为0.02，0.04，0.12美元/GB。

- Anycast IPs：每个容器1个免费IPv4和无限制的免费IPv6，然后额外的IPv4每月2美元。

- 证书：10个免费证书。

## 使用注意事项

- 只能通过官方的cli工具flyctl进行部署，可以从GitHub release下载：[https://github.com/superfly/flyctl/releases](https://github.com/superfly/flyctl/releases "flyctl release")。

- 可以通过本地Dockerfile、Docker Hub的公共镜像以及其他方式进行部署，但推荐使用公共镜像，因为使用Dockerfile的话，它会自动创建额外的容器进行镜像的构建，这个容器大概率是dedicated-cpu-4x的配置，不在免费额度之内，虽说构建的时间短，构建完毕会自动deactive，但还是有扣费风险，所以白嫖党要千万注意。

## 常用命令

    # 针对某个应用
    flyctl init #生成fly.toml，可修改后再部署
    flyctl deploy
    flyctl status
    flyctl logs
    flyctl regions list
    flyctl ips list
    flyctl scale show

    flyctl regions add some-region
    flyctl regions set some-region


    ## flyctl help
    
    Usage:
    flyctl [command]

    Available Commands:
    apps        Manage apps
    auth        Manage authentication
    autoscale   Autoscaling app resources
    builds      Work with Fly builds
    certs       Manage certificates
    checks      Manage health checks
    config      Manage an app's configuration
    dashboard   Open web browser on Fly Web UI for this app
    deploy      Deploy an app to the Fly platform
    destroy     Permanently destroys an app
    dns-records Manage DNS records
    docs        View Fly documentation
    domains     Manage domains
    help        Help about any command
    history     List an app's change history
    info        Show detailed app information
    init        Initialize a new application
    ips         Manage IP addresses for apps
    launch      Launch a new app
    list        Lists your Fly resources
    logs        View app logs
    monitor     Monitor deployments
    move        Move an app to another organization
    open        Open browser to current deployed application
    orgs        Commands for managing Fly organizations
    platform    Fly platform information
    postgres    Manage postgres clusters
    regions     Manage regions
    releases    List app releases
    restart     Restart an application
    resume      Resume an application
    scale       Scale app resources
    secrets     Manage app secrets
    ssh         Commands that manage SSH credentials
    status      Show app status
    suspend     Suspend an application
    version     Show version information for the flyctl command
    vm          Commands that manage VM instances
    volumes     Volume management commands
    wireguard   Commands that manage WireGuard peer connections

    Flags:
    -t, --access-token string   Fly API Access Token
    -h, --help                  help for flyctl
    -j, --json                  json output
    -v, --verbose               verbose output

    Use "flyctl [command] --help" for more information about a command.


*本文仅供参考，详见文档：[https://fly.io/docs/](https://fly.io/docs/ "fly.io docs")*