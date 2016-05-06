---
layout: post
title: 关于linux 进程
category: 技术
tags: process
keywords: 
description: 没什么
---

#linux进程
##关于进程线程的概念推荐文章
[进程、线程及其内存模型](http://buptjz.github.io/2014/04/23/processAndThreads)
[ Linux进程创建](http://blog.csdn.net/zhangzhebjut/article/details/39034327)
[Linux进程地址空间 && 进程内存布局](http://blog.csdn.net/yusiguyuan/article/details/45155035)

------------------------------------------------------------------------------

[引用自](http://blog.csdn.net/zhangzhebjut/article/details/39034327)
``` c
 PPID   PID  PGID   SID TTY      TPGID STAT   UID   TIME COMMAND
    0     1     1     1 ?           -1 Ss       0   0:01 /sbin/init
    0     2     0     0 ?           -1 S        0   0:00 [kthreadd]
    2     3     0     0 ?           -1 S        0   0:03 [ksoftirqd/0]
    2     5     0     0 ?           -1 S<       0   0:00 [kworker/0:0H]
    2     7     0     0 ?           -1 S        0   0:43 [rcu_sched]
    2     8     0     0 ?           -1 S        0   0:00 [rcu_bh]
    2     9     0     0 ?           -1 S        0   0:02 [migration/0]
    2    10     0     0 ?           -1 S        0   0:01 [watchdog/0]
    2    11     0     0 ?           -1 S        0   0:01 [watchdog/1]
    2    12     0     0 ?           -1 S        0   0:00 [migration/1]
    2    13     0     0 ?           -1 S        0   0:01 [ksoftirqd/1]
    2    15     0     0 ?           -1 S<       0   0:00 [kworker/1:0H]
    2    16     0     0 ?           -1 S        0   0:01 [watchdog/2]
    2    17     0     0 ?           -1 S        0   0:00 [migration/2]
    2    18     0     0 ?           -1 S        0   0:00 [ksoftirqd/2]
    2    20     0     0 ?           -1 S<       0   0:00 [kworker/2:0H]
    2    21     0     0 ?           -1 S        0   0:01 [watchdog/3]
    2    22     0     0 ?           -1 S        0   0:00 [migration/3]
    2    23     0     0 ?           -1 S        0   0:00 [ksoftirqd/3]
    2    25     0     0 ?           -1 S<       0   0:00 [kworker/3:0H]
    2    26     0     0 ?           -1 S<       0   0:00 [khelper]
    2    27     0     0 ?           -1 S        0   0:00 [kdevtmpfs]
```
TPGID一栏写着-1的都是没有控制终端的进程，也就是守护进程。在COMMAND一列用[]括起来的名字表示内核线程，这些线程在内核里创建，没有用户空间代码，因此没有程序文件名和命令行，通常采用以k开头的名字，表示Kernel。init是祖先进程，守护进程通常采用以d结尾的名字，表示Daemon。

附:由于在Linux中，每一个系统与用户进行交流的界面称为终端，每一个从此终端开始运行的进程都会依附于这个终端，这个终端就称为这些进程的控制终端，当控制终端被关闭时，相应的进程都会自动关闭。但是守护进程却能够突破这种限制，它从被执行开始运转，直到整个系统关闭时才退出。如果想让某个进程不因为用户或终端或其他地变化而受到影响，那么就必须把这个进程变成一个守护进程。