---
layout: post
title: keepalived遇到的问题
category: [keepalived]
tags: [keepalived, high availability(HA)]
comments: true
date: 2017-10-02 21:52:20
updated:
---
### 1. keepalived日志报错狂刷：IPVS: Can't initialize ipvs: Protocol not available

原因是`ip_vs`模块系统默认没有自动加载，可以通过`lsmod | grep ip_vs` 命令查看一下，如果没有任何输出则表示ip_vs模块并没有被内核加载，那必须手动加载一下：
<!--more -->
```
modprobe ip_vs
modprobe ip_vs_wrr

```
然后再查看系统日志发现keepalived已经正常工作了。如果要让系统开机加载此模块的话得讲刚才那两句话写到`/etc/rc.local`文件中，这样开机就能自动加载了。
### 2. 报错IPVS: Can't initialize ipvs: Permission denied (you must be root)
在`container`运行时加上`--privileged=true`