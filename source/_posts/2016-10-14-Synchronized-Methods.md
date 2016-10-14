---
title: Synchronized Methods
comments: false
date: 2016-10-14 22:02:26
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

## Synchronized Methods
Synchronization 提供了两种策略
1. Synchronized Methods
2. Synchronized Statements

本文介绍 Synchronized Methods

## 一个例子
使用 `synchronized` 关键字，使一个方法为 synchronized

```java
public class SynchronizedCounter {
    private int c = 0;

    public synchronized void increment() {
        c++;
    }

    public synchronized void decrement() {
        c--;
    }

    public synchronized int value() {
        return c;
    }
}
```

添加 `synchronized` 关键字有两个作用
1. 当一个 synchronized 方法调用并正在执行时，其他线程调用的 synchronized 方法将会阻塞，直到第一个线程下的 synchronized 方法执行结束
2. 当一个 synchronized 方法执行结束后，会为之后的所有阻塞的 synchronized 方法自动建立 `happens-before` 关系。这保证了该对象的所有状态的改变，对于所有线程是可见的

BTW：为构造函数添加 `synchronized` 是语法错误。因为毫无意义。
