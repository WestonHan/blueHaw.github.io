---
layout: post
title: unistall docker
comment: true
date: 2017-10-02 19:38:29
updated:
category: [docker]
tags: [docker]
---

```bash
yum list installed | grep docker
yum -y remove docker-engine.x86_64
rm -rf /var/lib/docker
```