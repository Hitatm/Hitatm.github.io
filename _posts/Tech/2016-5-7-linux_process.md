---
layout: post
title: 关于linux 进程
category: Tech
tags: process:进程
keywords: 
description: 没什么
---

#  linux进程

##  关于进程线程的概念推荐文章
[进程、线程及其内存模型](http://buptjz.github.io/2014/04/23/processAndThreads)

[ Linux进程创建](http://blog.csdn.net/zhangzhebjut/article/details/39034327)

[Linux进程地址空间 && 进程内存布局](http://blog.csdn.net/yusiguyuan/article/details/45155035)


------------------------------------------------------------------------------

[引用自](http://blog.csdn.net/zhangzhebjut/article/details/39034327)

``` C
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

## 操作进程的工具

1. strace 打印一个程序和它的子进程调用的每个系统调用的轨迹。 用-static 编译一个程序，能够得到更清楚的轨迹
2. ps 列出系统中当前的进程（包括僵尸进程）
3. top 打印出关于当前进程的资源使用情况
4. kill 发送一个信号给进程。
5. /proc 一个虚拟文件系统，以ASCII文本格式输出大量内核数据的内容，用户程序可以读取。

```C
#include<stdio.h>
#include<stdlib.h>
#include<fcntl.h>
#include<unistd.h>
void handler();
int main(int argc,char* argv[],char*envp[])
{
 int i=0;
 printf("%s,%s\n",argv[0],argv[1]);
 return 0;
}
```
strace test

```C
execve("/usr/bin/test", ["test"], [/* 72 vars */]) = 0
brk(0)                                  = 0x8dfd000
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
mmap2(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xb773d000
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
open("/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat64(3, {st_mode=S_IFREG|0644, st_size=108559, ...}) = 0
mmap2(NULL, 108559, PROT_READ, MAP_PRIVATE, 3, 0) = 0xb7722000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
open("/lib/i386-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\1\1\1\0\0\0\0\0\0\0\0\0\3\0\3\0\1\0\0\0\340\233\1\0004\0\0\0"..., 512) = 512
fstat64(3, {st_mode=S_IFREG|0755, st_size=1754876, ...}) = 0
mmap2(NULL, 1759868, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0xb7574000
mmap2(0xb771c000, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1a8000) = 0xb771c000
mmap2(0xb771f000, 10876, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0xb771f000
close(3)                                = 0
mmap2(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xb7573000
set_thread_area({entry_number:-1 -> 6, base_addr:0xb7573940, limit:1048575, seg_32bit:1, contents:0, read_exec_only:0, limit_in_pages:1, seg_not_present:0, useable:1}) = 0
mprotect(0xb771c000, 8192, PROT_READ)   = 0
mprotect(0x8050000, 4096, PROT_READ)    = 0
mprotect(0xb7760000, 4096, PROT_READ)   = 0
munmap(0xb7722000, 108559)              = 0
brk(0)                                  = 0x8dfd000
brk(0x8e1e000)                          = 0x8e1e000
open("/usr/lib/locale/locale-archive", O_RDONLY|O_LARGEFILE|O_CLOEXEC) = 3
fstat64(3, {st_mode=S_IFREG|0644, st_size=8752496, ...}) = 0
mmap2(NULL, 2097152, PROT_READ, MAP_PRIVATE, 3, 0) = 0xb7373000
mmap2(NULL, 4096, PROT_READ, MAP_PRIVATE, 3, 0x855000) = 0xb773c000
close(3)                                = 0
close(1)                                = 0
close(2)                                = 0
exit_group(1)                           = ?
+++ exited with 1 +++


```

<!-- ## 进程 -->