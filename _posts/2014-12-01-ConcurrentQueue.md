---
layout: post
title: 阻塞和非阻塞并发队列算法
description:"我们将描述两个具有实践性的算法，一个非阻塞的并发队列算法，以及另一个具有入队锁和出队锁的阻塞算法。"
modified: 2014-12-01
tags: [algorithms]
---

###简介
&emsp;&emsp;并发FIFO队列在并行程序以及操作系统中有广泛的应用，为了确保程序的正确性，在并发访问共享队列的时候必须进行同步化。举个例子，比如在生产者与消费者模型的应用中，在没有产品时，消费者就必须要等到生产者生产出产品后才能运行。对于像FIFO队列这样的并发数据结构的算法，一般分成两种：阻塞和非阻塞，阻塞算法使用一个慢的或延迟的进程去避免更快进程在不确定的共享内存中完成操作，这个慢的进程可以看作锁。非阻塞算法保证在一个或多个活跃进程尝试操作共享数据结构的时候，某些操作将在有限的时间周期内完成，不会发生因为像抢锁所形成的死锁情况。在同步多进程系统中，尤其是多进程的情况，阻塞算法中，当一个进程在不适当的时间被挂起或延迟的时候，阻塞算法的性能将会严重下降。延迟可能的原因有进程的抢占式调度，页面中断(引起页面置换)，以及缓存未命中。但是，在面对以上问题，非阻塞算法鲁棒性更好。