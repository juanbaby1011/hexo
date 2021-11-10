---
title: linux 进程管理
tags:
  - linux
categories:
  - linux
keywords:
  - linux进程管理
type: 原创
toc: true
date: 2021-11-09 17:09:43
updated: 2021-11-09 17:09:43
---

# ps 进程查看

## 查看所有进程

显示所有状态

> ps -aux

## 查看指定程序进程

比较常用的命令

> ps -ef |grep tomcat

## 查看当前用户相关进程信息

> ps aux | more

## 查看指定进程

注意，如果有找到进程，则在当前一行的下方出现进程相关的内容。
如果没有找到，则如终会有多一行的信息被展示出来。
可以拿tomcat 试试。

> ps -aux | grep tomcat

## 查看精确运行时间

> ps -eo pid,lstart,etime,cmd | grep 20350

## 字段含义解释

> ps -ef

字段含义如下：
> UID       PID   PPID     C STIME   TTY    TIME     CMD
> root     18887 18828   0  08:09     pts/0    00:00:00    grep ApacheJetspeed

UID 程序被该 UID 所拥有
PID 就是这个程序的 ID
PPID 则是其上级父程序的ID
C CPU 使用的资源百分比
STIME 系统启动时间
TTY 登入者的终端机位置
TIME 使用掉的 CPU 时间。
CMD 所下达的指令为何

## 精确动行时间

精确时间和启动后所流逝的时间

命令
> ps -eo pid,lstart,etime,cmd | grep 20350

结果如下
> 20350 Wed Jun 30 15:57:11 2021 16-00:00:48 java -Xms18g -Xmx18g 省略...

# kill 杀死进程

通过 pid 发送终止信号，终止进程。

## 彻底杀死进程

最常用的操作，比较暴力，生产环境如果可以确定强制杀死进程不影响数据正确性，可以用`-9`，否则一般使用`kill -15`

> kill -9 pid

## 杀死所有用户进程

这个算是一个大招，有效。不过自己也被杀死了。
kill -9 $(ps -ef | grep redis)

## 正确的退出程序

> kill -15 pid

这种方式有可能被阻塞式，会杀掉相关联的进程，用kill -9会出现关联进程残留的问题。
操作系统会发送一个 SIGTERM 信号给对应的程序，程序接收到后：

1. 当前程序立刻停止；
2. 程序释放相应资源，然后再停止；
3. 程序可能仍然继续运行。

## 查看所有信号

> kill -l    # 有64个信号

只有第9种信号(SIGKILL)才可以无条件终止进程，其他信号进程都有权利忽略。

```console
 1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL
 5) SIGTRAP      6) SIGABRT      7) SIGBUS       8) SIGFPE
 9) SIGKILL     10) SIGUSR1     11) SIGSEGV     12) SIGUSR2
13) SIGPIPE     14) SIGALRM     15) SIGTERM     16) SIGSTKFLT
17) SIGCHLD     18) SIGCONT     19) SIGSTOP     20) SIGTSTP
21) SIGTTIN     22) SIGTTOU     23) SIGURG      24) SIGXCPU
...
```

## 9种kill 信号

```console
HUP     1    终端断线
INT     2    中断（同 Ctrl + C）
QUIT    3    退出（同 Ctrl + \）
TERM   15    终止
KILL    9    强制终止
CONT   18    继续（与STOP相反， fg/bg命令）
STOP   19    暂停（同 Ctrl + Z）
```
