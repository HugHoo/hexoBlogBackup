---
title: Thread Interference
comments: false
date: 2016-10-14 22:00:18
tags:
- Java
- Concurrency
- Thread
- Synchronization
- Interference
categories:
- Java
- Concurrency
- Thread
---

## Thread interference
Interference happens when two operations, running in different threads,
but acting on the same data, interleave.  
当两个运行在不同线程的操作，作用在同一个数据上，会发生线程干扰 (Thread interference)

This means that the two operations consist of multiple steps, and the sequences of steps overlap.  
这也意味着，两个操作分别由多个步骤组成，且两个操作的步骤序列交叠（导致 Thread interference)

## 一个例子

这里有一个类 `Counter`

```java
class Counter {
    private int c = 0;

    public void increment() {
        c++;
    }

    public void decrement() {
        c--;
    }

    public int value() {
        return c;
    }
}
```

看起来，`Counter`中的操作不会产生交叠。比如`increment()`方法中，唯一的 statement 是`c++`。

然而，即使是一个简单的 statement，在JVM中也会转换为多个步骤 —— 即该 statement 非原子操作

一个简单的 statement `c++`，可以简单分为三步 ( `c--`同理 )
1. 取出当前`c`的值
2. 将取出的值增加1
3. 将取出的值存储至`c`（覆盖原值）

假设，线程A调用`increment()`的同时，线程B调用`decrement()`。则两个操作可能产生如下交叠
1. Thread A: 取出 c.
2. Thread B: 取出 c.
3. Thread A: 将取出的值增加 1.
4. Thread B: 将取出的值减少 -1.
5. Thread A: 将取出的值存储至 c; c == 1.
6. Thread B: 将取出的值存储至 c; c == -1.

线程A的结果丢失，被线程B的结果覆盖。这是一种可能的结果，也可能线程B的结果被A覆盖；或没有交叠，
不发生错误 —— Thread interference 结果**不可预料**，bugs很难被发现并且修复
