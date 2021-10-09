---
title: ArrayList、LinkedList和Vector的区别
date:
category:
  - [面试经验, Java]
tags:
  - List
  - 容器
---

**ArrayList、LinkedList 和 Vector 的区别**

### 一、区别

> 相同点

这三个类都实现了 Collection 接口，存储数据的特点相同：存储有序的、可重复的数据

> 不同点

1. ArrayList 作为 List 接口的主要实现类，线程不安全，效率高，底层使用 Object[] elementData 存储
2. LinkedList 对于频繁的插入、删除操作，使用此类效率比 ArrayList 高，底层使用双向链表存储
3. Vector 作为 List 接口古老实现类，线程安全，效率低，底层使用 Object[] elementData 存储

### 二、源码分析

- ArrayList

```java
JDK7:
ArrayList list = new ArrayList();	// 底层创建了长度是10的Object[]数组elementData
list.add(123);	// elementData[0] = new Integer(123);
...
list.add(11);	// 如果此次的添加导致底层elementData数组容量不够，则扩容。
// 默认情况下，扩容为原来容量的1.5倍，同时将原有数组中的数据复制到新的数组中。
// 建议：如果知道存储的元素数量，使用带参数的构造器，避免自动扩容，提高效率
ArrayList list = new ArrayList(int capacity);

JDK8：
ArrayList list = new ArrayList();	// 底层Object[]数组elementData初始化为{}，并没有创建长度为10的数组
list.add(123);	// 第一次调用add()方法时，底层才创建了长度是10数组，并将数据123添加到elementData中
// 后续添加和扩容操作和JDK7无异

// jdk7中ArrayList的创建类似于单例的饿汉式，而jdk8中的ArrayList的创建类似于单例的懒汉式，延迟了数组的创建，节省了内存。
```

- LinkedList

```java

LinkedList list = new LinkedList();	// 内部声明了Node类型的first和last属性，默认值是null
list.add(123)	// 将123添加到Node中，创建了Node对象
// 其中，Node定义为：即双向链表结构
    private static class Node<E> {
        E item;
        Node<E> next;
        Node<E> prev;

        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.next = next;
            this.prev = prev;
        }
    }
```

- Vector

```java
// jdk7和jdk8中通过Vector()构造器创建对象时，底层都创建了长度为10的数组；
// 在扩容方面，默认扩容为原来数组的2倍
```
