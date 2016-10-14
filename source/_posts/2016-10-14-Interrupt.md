---
title: Interrupt
comments: false
date: 2016-10-14 21:53:01
categories:
- Java
- Concurrency
- Thread
tags:
- Java
- Concurrency
- Thread
- Interrupt
---

## Interrupt ?
> An interrupt is an indication to a thread that it should stop what it is doing and do something else.
>
> interupt 是一个指示，指示一个线程停止正在做的事情，并做一些其他的事情。

It's up to the programmer to decide exactly how a thread responds to an interrupt  
程序员决定一个线程该在 interrupt时应该做什么

But it is **very common** for the thread to terminate  
我们通常使用 Interrupt去终止线程

## How to use ?

调用 `interrupt()`，向线程发送 interrupt指示。为了使 interrupt机制运行正确，线程必须支持自己的 interrupt

## Supportting Interruption
If the thread is frequently invoking methods that throw InterruptedException, it simply returns from the run method after it catches that exception.  
如果一个线程内，频繁的调用一个可以 `throw InterruptedException`的方法，只需要在 `catch`中 `return`,即可退出`run()` —— 即终止了线程。

```java
for (int i = 0; i < importantInfo.length; i++) {
    // Pause for 4 seconds
    try {
        Thread.sleep(4000);
    } catch (InterruptedException e) {
        // We've been interrupted: no more messages.
        return;
    }
    // Print a message
    System.out.println(importantInfo[i]);
}
```

Many methods that throw InterruptedException, such as sleep, are designed to cancel their current operation and return immediately when an interrupt is received.  
`Thread`的很多方法都可以`throw InterruptedException`，比如`sleep()`。当获取到`interrupt`指示时，这些方法可以停止当前的操作，并立即返回（终止线程）

What if a thread goes a long time without invoking a method that throws InterruptedException? Then it must periodically invoke Thread.interrupted, which returns true if an interrupt has been received.  
如果一个线程会运行很长时间，且没有调用任何可以`throw InterruptedException`的方法，怎么办？必须定期运行`Thread.interrupted()`方法，当获取`interrupt`指令时返回`true`

```java
for (int i = 0; i < inputs.length; i++) {
    heavyCrunch(inputs[i]);
    if (Thread.interrupted()) {
        // We've been interrupted: no more crunching.
        return;
    }
}
```

如果项目比较复杂的话，`throw new InterruptedException`更有意义

```java
if (Thread.interrupted()) {
    throw new InterruptedException();
}
```

## The Interrupt Status Flag
The interrupt mechanism is implemented using an internal flag known as the interrupt status. Invoking Thread.interrupt sets this flag. When a thread checks for an interrupt by invoking the static method **Thread.interrupted**, interrupt status is cleared. The non-static **isInterrupted** method, which is used by one thread to query the interrupt status of another, does not change the interrupt status flag.  

`interrupt`机制的实现，使用了一个内部的flag，用于标识`interrupt status`。  
调用 **静态方法**  `Thread.interrupted()`（用于检查当前`Thread`是否`interrupt`），`interrupt status`会被清除。  
调用 **非静态方法**  `isInterrupted()`（用于一个`Thread`查询另一个`Thread`是否`interrupt`），不会清除`interrupt status`

## Reference
- [Interrupts](http://docs.oracle.com/javase/tutorial/essential/concurrency/interrupt.html)
