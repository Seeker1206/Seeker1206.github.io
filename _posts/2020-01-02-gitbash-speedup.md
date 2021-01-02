---
layout:     post
title:      "git bash下载加速"
subtitle:   "如何让你的git下载飞快"
date:       2021-01-02 16:26:00
author:     "Seeker"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - 技术
    - git
    - 代理
---

# 正文

因为众所周知的一些原因，git在大陆访问速度极慢，直连情况下下载速度我这里只有100kB+/s，简直不能忍。

好在github支持配置代理，当然前提是你自己要有能用的梯子。

    # 配置http协议代理，端口号改成你自己的本地代理的端口号
    git config --global http.proxy http://127.0.0.1:1081
    git config --global https.proxy https://127.0.0.1:1081

    # 查看当前所有config
    git config -l
    # 取消代理
    git config --global --unset http.proxy
    git config --global --unset https.proxy

上面这样会把所有的http协议都通过代理，如果你本地有其他的git仓库，可以只通过下面的方式来只给到github的http流量走代理。

    # 同样把端口号换成本地代理的端口号
    git config --global http.https://github.com.proxy https://127.0.0.1:1081
    git config --global https.https://github.com.proxy https://127.0.0.1:1081

# Reference

1、[一招 git clone 加速](https://juejin.cn/post/6844903862961176583)
