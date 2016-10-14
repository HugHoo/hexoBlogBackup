---
title: Processes & Threads
comments: false
date: 2016-10-14 21:39:20
categories:
- Java
- Concurrency
tags:
- Java
- Concurrency
- Thread
---

## Process ?

> A process has a self-contained execution environment.A process generally has a complete, private set of basic run-time resources;in particular, each process has its own memory space.
> 
> 一个进程，拥有一个独用的的执行环境。一个进程通常有一组完整的，私有的，基本的运行时资源。
特别的，每个进程拥有自己的内存空间

进程通常被看做一个程序或应用的代名词。其实，用户看到的单个应用，事实上可能是一组协作的进程。

为了促进进程间的通信，大多数操作系统支持 Inter Process Communication (IPC) resources,
例如 pipes, sockets

IPC 不仅用来在同一个系统上的进程间的通信，也可以用在不同系统上进程间通信

## Thread ?

> Threads are sometimes called lightweight processes.Both processes and threads provide an execution environment, but creating a new thread requires fewer resources than creating a new process.
> 
> 线程，有时候称之为“轻量级进程”。线程和进程都提供单独的执行环境，但新建一个线程所需的资源要少于
新建一个进程所需的资源。

Threads exist within a process — every process has at least one.  
线程一定包含在进程中。每个进程至少有一个线程。

Threads share the process's resources, including memory and open files. This makes for efficient, but potentially problematic, communication.  
线程可以共享进程的资源，包括内存和打开的文件。这么做是可以更高效，但会存在一些问题

But from the application programmer's point of view, you start with just one thread, called the main thread. This thread has the ability to create additional threads

## Different & Contact ?
- 线程一定包含在进程中。每个进程至少有一个线程。
- 进程通常有一组完整的，私有的，基本的运行时资源。  
线程可以共享进程的资源。

## Reference
- [Processes & Threads](https://docs.oracle.com/javase/tutorial/essential/concurrency/procthread.html)
