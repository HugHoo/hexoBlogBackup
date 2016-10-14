---
title: Synchronization
comments: false
date: 2016-10-14 21:56:42
tags:
- Java
- Concurrency
- Thread
- Synchronization
categories:
- Java
- Concurrency
- Thread
---

## Synchronization

线程通信，主要通过共享访问进程资源。这种通信方式非常高效，但存在两个问题：**thread interference** & **memory consistensy** errors

解决这两种问题的工具就是 Synchronization

然而，Synchronization 会引入 Thread contention。当多个线程试图同时访问同一个资源时，Synchronization 会导致一个或几个线程执行变得更慢，甚至 suspend 线程的执行。

## This section covers the following topics:

- **Thread Interference** describes how errors are introduced when multiple threads access shared data.
- **Memory Consistency** Errors describes errors that result from inconsistent views of shared memory.
- **Synchronized Methods** describes a simple idiom that can effectively prevent thread interference and memory consistency errors.
- **Implicit Locks and Synchronization** describes a more general synchronization idiom, and describes how synchronization is based on implicit locks.
- **Atomic Access** talks about the general idea of operations that can't be interfered with by other threads.
