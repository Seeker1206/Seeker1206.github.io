---
layout:     post
title:      "什么是伪共享"
subtitle:   "false sharing"
date:       2021-06-27 17:28:00
author:     "Seeker"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - cache
    - false sharing
    - linux
---

# 正文
伪共享(false sharing)指的是，在多核SMP系统中，当不同的核尝试频繁读写共享cache上同一cache line的数据时，就会引发频繁的cache更新，进而影响程序性能。

下图是一个经典的例子。同一进程中的不同线程经常会有频繁读写相邻内存区域的情况发生，当线程A和线程B恰好被调度到不同的core上，且线程A访问的变量X和线程B访问的变量Y切好处于同一cache line上。那么当A改动了X的值，则core2 L1、L2的cache line都要被失效，当线程B想要改动Y的值时，需要从L3上去读取。反过来也一样，而且频繁的刷新、失效cache都会带来一定的开销，这就造成“伪共享”情况。

![img](/img/falsesharing/example.png)

那么如何避免这种情况呢，其实也很简单，对于可能发生上述情景的相邻数据，声明时强制对其到缓存行就可以了。
linux在cache.h中存在如下定义：

    #ifndef ____cacheline_aligned
    #define ____cacheline_aligned __attribute__((__aligned__(SMP_CACHE_BYTES)))
    #endif

    #ifndef ____cacheline_aligned_in_smp
    #ifdef CONFIG_SMP
    #define ____cacheline_aligned_in_smp ____cacheline_aligned
    #else
    #define ____cacheline_aligned_in_smp
    #endif /* CONFIG_SMP */
    #endif

当定义了SMP时，可使用____cacheline_aligned_in_smp使变量强制对齐到缓存行大小，从而避免伪共享问题。