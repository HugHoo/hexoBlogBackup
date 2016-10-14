---
title: Intrinsic Lock & Synchronization
comments: false
date: 2016-10-14 22:04:16
tags:
- Java
- Concurrency
- Thread
- Synchronization
- Intrinsic lock
categories:
- Java
- Concurrency
- Thread
---

## Intrinsic Locks and Synchronization

> Synchronization is built around an internal entity known as the intrinsic lock or monitor lock. (The API specification often refers to this entity simply as a "monitor.") Intrinsic locks play a role in both aspects of synchronization: enforcing exclusive access to an object's state and establishing happens-before relationships that are essential to visibility.

同步机制的实现，是围绕被称为 intrinsic lock 的内部实例实现的。Intrinsic lock 在同步机制中发挥两个作用：强制独占访问对象状态的权限，并建立可见的 happens-before 关系

> Every object has an intrinsic lock associated with it. By convention, a thread that needs exclusive and consistent access to an object's fields has to acquire the object's intrinsic lock before accessing them, and then release the intrinsic lock when it's done with them. A thread is said to own the intrinsic lock between the time it has acquired the lock and released the lock. As long as a thread owns an intrinsic lock, no other thread can acquire the same lock. The other thread will block when it attempts to acquire the lock.

每一个对象都有一个 intrinsic lock 与之关联。一般的，一个需要独占某对象的访问权限的线程，在访问该对象的资源之前，需要请求这个对象的 intrinsic lock，并在访问结束后释放该 intrinsic lock 。在获取 lock 到释放 lock 这之间，该线程拥有该 intrinsic lock 。只要一个线程拥有 intrinsic lock，其他线程将不会获取该 lock 。当试图请求一个已经被独占的 lock 时，其他线程将会 block 。

> When a thread releases an intrinsic lock, a happens-before relationship is established between that action and any subsequent acquisition of the same lock.

当一个线程释放 intrinsic lock，该动作将会与随后的获取 lock 的请求之间，建立一个 happens-before 关系 —— 即随后的被阻塞的线程，可以得知 lock 已被释放。

## Locks In Synchronized Methods

> When a thread invokes a synchronized method, it automatically acquires the intrinsic lock for that method's object and releases it when the method returns. The lock release occurs even if the return was caused by an uncaught exception.

当一个线程调用 synchronized method，将自动获取 intrinsic lock，然后在方法 return 时释放 lock 。即使 method 是因为没有 catch 的 exception 而 return, 也会释放 lock

> You might wonder what happens when a static synchronized method is invoked, since a static method is associated with a class, not an object. In this case, the thread acquires the intrinsic lock for the Class object associated with the class. Thus access to class's static fields is controlled by a lock that's distinct from the lock for any instance of the class.

关于 static synchronized method 的调用对 lock 的请求 ↑↑↑