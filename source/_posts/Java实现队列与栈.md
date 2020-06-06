---
title: Java实现队列与栈
date: 2020-06-06 11:41:16
tags: 数据结构
---

## 为什么不用Stack

Java中有实现栈的Stack，但是官方不推荐使用了，而是推荐使用双端队列Deque来实现栈与队列，这部分在官方文档Stack的描述是这样的：

> A more complete and consistent set of LIFO stack operations is provided by the [`Deque`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html) interface and its implementations, which should be used in preference to this class. For example:
>
> ```java
> Deque<Integer> stack = new ArrayDeque<Integer>();
> ```

究其根本，这是跟Vector，HashTable一样，是一个历史的遗留问题，在我看来，它被摒弃不用主要有以下几点原因：

<!--more-->

* Stack底层是数组实现的，这也就存在着扩容的问题。
* 它继承自Vector，而从JDK2.0开始，Vector在不要求线程安全的情况下，可以使用ArrayList替代，即便是在要求线程安全的时候可以用 [`Collections.synchronizedList`](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#synchronizedList-java.util.List-)来装饰，所以Vector基本上摒弃了，那Stack自然也就不推荐使用了。
* Stack继承自Vector，Stack可以复用Vector的方法，从数据结构的角度来说，栈与数组不应该是继承的关系，这样的设计本身就不严谨。

## 双端队列 Deque

双端队列，顾名思义就是可以对队列的始末位置进行插入、删除、查询的操作，每一种方法又按照对操作失败时不同的处理分为两类：抛异常跟返回特殊值（null或者false）。Deque一共有12种方法。关于双端队列的使用，在['官方文档'](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html)中写的非常详细：

> The twelve methods described above are summarized in the following table:
>
> Summary of Deque methods
>
> |             | **First Element (Head)**                                     | **First Element (Head)**                                     | **Last Element (Tail)**                                      | **Last Element (Tail)**                                      |
> | ----------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
> |             | *Throws exception*                                           | *Special value*                                              | *Throws exception*                                           | *Special value*                                              |
> | **Insert**  | [`addFirst(e)`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#addFirst-E-) | [`offerFirst(e)`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#offerFirst-E-) | [`addLast(e)`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#addLast-E-) | [`offerLast(e)`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#offerLast-E-) |
> | **Remove**  | [`removeFirst()`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#removeFirst--) | [`pollFirst()`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#pollFirst--) | [`removeLast()`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#removeLast--) | [`pollLast()`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#pollLast--) |
> | **Examine** | [`getFirst()`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#getFirst--) | [`peekFirst()`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#peekFirst--) | [`getLast()`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#getLast--) | [`peekLast()`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#peekLast--) |



### 实现队列：

> This interface extends the [`Queue`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html) interface. When a deque is used as a queue, FIFO (First-In-First-Out) behavior results. Elements are added at the end of the deque and removed from the beginning. The methods inherited from the `Queue` interface are precisely equivalent to `Deque` methods as indicated in the following table:
>
> Comparison of Queue and Deque methods
>
> | **`Queue` Method**                                           | **Equivalent `Deque` Method**                                |
> | ------------------------------------------------------------ | ------------------------------------------------------------ |
> | [`add(e)`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html#add-E-) | [`addLast(e)`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#addLast-E-) |
> | [`offer(e)`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html#offer-E-) | [`offerLast(e)`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#offerLast-E-) |
> | [`remove()`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html#remove--) | [`removeFirst()`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#removeFirst--) |
> | [`poll()`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html#poll--) | [`pollFirst()`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#pollFirst--) |
> | [`element()`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html#element--) | [`getFirst()`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#getFirst--) |
> | [`peek()`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html#peek--) | [`peekFirst()`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#peek--) |

实现队列的时候，**元素在Deque的末端添加元素，在Deque的始端删除元素**，这样就实现了FIFO。

方便理解，将上述的方法分为两类：

* *Throws exception*：

  * | **`Queue` Method**                                           | **Equivalent `Deque` Method**                                |
    | ------------------------------------------------------------ | ------------------------------------------------------------ |
    | [`add(e)`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html#add-E-) | [`addLast(e)`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#addLast-E-) |
    | [`remove()`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html#remove--) | [`removeFirst()`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#removeFirst--) |
    | [`element()`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html#element--) | [`getFirst()`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#getFirst--) |

* *Special value*：

  * | **`Queue` Method**                                           | **Equivalent `Deque` Method**                                |
    | ------------------------------------------------------------ | ------------------------------------------------------------ |
    | [`offer(e)`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html#offer-E-) | [`offerLast(e)`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#offerLast-E-) |
    | [`poll()`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html#poll--) | [`pollFirst()`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#pollFirst--) |
    | [`peek()`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html#peek--) | [`peekFirst()`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#peek--) |

### 实现栈：

> Deques can also be used as LIFO (Last-In-First-Out) stacks. This interface should be used in preference to the legacy [`Stack`](https://docs.oracle.com/javase/8/docs/api/java/util/Stack.html) class. When a deque is used as a stack, elements are pushed and popped from the beginning of the deque. Stack methods are precisely equivalent to `Deque` methods as indicated in the table below:
>
> Comparison of Stack and Deque methods
>
> | **Stack Method**                                             | **Equivalent `Deque` Method**                                |
> | ------------------------------------------------------------ | ------------------------------------------------------------ |
> | [`push(e)`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#push-E-) | [`addFirst(e)`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#addFirst-E-) |
> | [`pop()`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#pop--) | [`removeFirst()`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#removeFirst--) |
> | [`peek()`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#peek--) | [`peekFirst()`](https://docs.oracle.com/javase/8/docs/api/java/util/Deque.html#peekFirst--) |

在实现栈的时候，**元素的插入跟删除都是在Deque的始端执行的**，这样就实现了LIFO。

### 总结

* 一个双端队列，根据使用方法的不同，可以将它看作是队列或是栈。

* 无论是队列还是栈，peek()方法都是用于查询。

* 虽然Deque允许插入空元素，但是不推荐这样做。因为在操作失败返回特殊值的时候，null值是用来表示Deque为空的。

