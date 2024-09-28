---
title: My Practical Experience On Linux
date: 2024-09-23 21:12:10
toc: true
tags:
    - Linux
categories: knowledge
excerpt: Writing about the Linux problems appeared in practice.
---
# Linux 内核相关
## 僵尸进程
### 原因
- 父进程没有回收子进程的资源
- 子进程的进程描述符仍然存在，因此子进程成为僵尸进程

### 解决方法
- 父进程调用 `wait()` 函数，回收子进程的资源
- 或者父进程调用 `waitpid()` 函数，回收子进程的资源
- 或者父进程调用 `signal(SIGCHLD,SIG_IGN)` 函数，忽略 `SIGCHLD` 信号，让内核自动回收子进程的资源

### 实际开发
当我们在 Linux 主机上遇到占用显存但是找不到对应的进程时（如下图案例所示），往往就是子进程所占用的资源没有被回收，而父进程已经结束。这种情况一般会在多线程并行当中出现，比如在多线程并行训练模型时，如果主线程在子线程结束之前就已经结束，那么子线程就会变成僵尸进程，占用显存。

<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/knowledge/81bc3c2f9222f65b84effed3bad9e68.png" alt="僵尸进程案例一" width="70%"/>

<img src="https://lzhms.oss-cn-hangzhou.aliyuncs.com/images/blog/knowledge/20240923212153.png" alt="僵尸进程案例二" width="70%"/>

对应的解决方法，一般可以通过检测僵尸进程，然后再寻找僵尸进程对应的父进程，安全地终止父进程后，操作系统就会自动清理对应的子进程。

+ 检测僵尸进程
```bash
ps aux | grep Z
```
在输出中，状态列（`STAT`）会显示为 `Z`，表示这是一个僵尸进程.

+ 查找僵尸进程对应的父进程
```bash
ps -o ppid= -p 2721429
```
可以通过以上命令，查找僵尸进程对应的父进程的 `PID`，然后进行终止.
+ 终止父进程
```bash
kill -9 2721429
```
当父进程被杀死后，操作系统会自动将这些僵尸进程的父进程设置为 `init` 进程（`PID 1`），相当于讲这些僵尸进程转移给了 `init` 进程作为接管的父进程。`init` 是系统的主进程，它会自动调用 `wait()` 来回收所有孤立的僵尸进程，从而清除它们。
