---
title: Memory Consistency Error
comments: false
date: 2016-10-14 22:01:19
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

## Memory Consistency Errors ?
> Memory consistency errors occur when different threads have inconsistent views of what should be the same data.  

当不同的线程对同一个数据有不一致的views时，产生内存一致性错误(Memory consistency errors)

产生 memory consistency error 的原因比较复杂，不过我们并不需要知道这其中的细节，只需要知道如何避免该错误。

要避免 memory consistency error，需要理解 **happens-before** 关系。这个关系是一个简单的保证，保证当内存被一个特定的 statement 修改时，对于另一个 statement 是可见的。

参考下边的例子，再回来理解上边的内容

## 一个例子

令一个属性 `couter` 声明并初始化

```java
int counter = 0;
```

这个 `counter` 被线程 A 和线程 B 共享

假定线程 A 对 `couter` 的操作为

```java
couter++;
```

假定线程 B 对 `couter` 的操作为

```java
System.out.println(couter);
```

如果上述两个操作在同一个线程，则可以肯定，最终输出的结果为1.但这两个操作分别在两个线程中执行，线程 B 最终输出的结果可能为0，因为无法保证线程 A 对 `couter` 的操作对于 B 是可见的 —— 即 B 无法知道 A 是否正在修改值

除非我们为 A 和 B 建立一个 `happens-before` 关系。

建立 `happens-before` 关系，方法就是 Synchronization。这在另一个文章中详谈。

---------

其实我们已经已经见过这种 `happens-before` 关系
- 当一个 statement 调用 `Thread.start` 时，之后的 statement 不仅和该 statement 有了 `happens-before` 关系，且和新线程中的 statement 有了 `happens-before` 关系 —— 即之后的 statment 已知一个新的线程在`run`
- `Thread.join` 同理。
