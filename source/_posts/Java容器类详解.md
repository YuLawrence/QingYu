---
title: Java容器类详解
date:
category:
  - [面试经验, Java]
tags:
  - Java容器类
  - 数据结构
---

> - 源于尚硅谷视频教程总结的笔记
> - 推荐去看视频教程：https://www.bilibili.com/video/BV1Kb411W75N

------

**集合、数组都是对多个数据进行存储操作的结构，简称Java容器**

> 先寄出这张经典老图

![image-20211005215546519](https://gitee.com/YuLawrence/picture/raw/master/202110/20211005_215826875_0.png)

------



> Java 容器类主要分为以下两类：

### 一、Collection

> 常用方法，注：向Collection接口的实现类的对象中添加数据obj时，一般要求obj所在类要重写equals()

- contains(Object obj)：判断当前集合是否包含obj
- containsAll(Collection list1)：判断list1中存在的元素是否都存在当前集合中
- remove(Object obj)：从当前集合中移除obj元素
- removeAll(Collection list1)：从当前集合中移除list1中的所有元素
- retainAll(Collection list1)：交集：获取当前集合和list1集合的交集，并返回给当前集合
- equals(Object obj)：判断当前形参和当前集合的元素是否相同，如果实现Collection的是ArrayList类，则顺序也要一致才会返回true；
- hashCode()：返回当前对象的哈希值
- toArray()：将集合转换为数组 ------------------------> 拓展：将数组转化为集合，调用Array类的静态方法asList()
- iterator()：返回iterator接口的实例，用于遍历集合的元素

#### 1.List：有序的、可重复的

- **ArrayList**

- **LinkedList**

- **Vector**

> List接口的常用方法

- void add(intindex, Object ele):在index位置插入ele元素
- boolean addAll(int index, Collection eles):从index位置开始将eles中的所有元素添加进来
- Object get(int index):获取指定index位置的元素
- int indexOf(Object obj):返回obj在集合中首次出现的位置
- int lastIndexOf(Object obj):返回obj在当前集合中末次出现的位置
- Object remove(int index):移除指定index位置的元素，并返回此元素
- Object set(int index, Object ele):设置指定index位置的元素为ele
- List subList(int fromIndex, int toIndex):返回从fromIndex到toIndex位置的子集合

```java
import org.junit.Test;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Iterator;
import java.util.List;

/**
 *
 * List接口的常用方法
 */
public class ListTest {
    /**
     *
     * void add(int index, Object ele):在index位置插入ele元素
     * boolean addAll(int index, Collection eles):从index位置开始将eles中的所有元素添加进来
     * Object get(int index):获取指定index位置的元素
     * int indexOf(Object obj):返回obj在集合中首次出现的位置
     * int lastIndexOf(Object obj):返回obj在当前集合中末次出现的位置
     * Object remove(int index):移除指定index位置的元素，并返回此元素
     * Object set(int index, Object ele):设置指定index位置的元素为ele
     * List subList(int fromIndex, int toIndex):返回从fromIndex到toIndex位置的子集合
     *
     * 总结：常用方法
     * 增：add(Object obj)
     * 删：remove(int index)：List接口重载的删除方法 / remove(Object obj)：collection接口中的
     * 改：set(int index, Object ele)
     * 查：get(int index)
     * 插：add(int index, Object ele)
     * 长度：size()
     * 遍历：① Iterator迭代器方式
     *      ② 增强for循环
     *      ③ 普通的循环
     *
     */

    @Test
    public void test3(){
        ArrayList list = new ArrayList();
        list.add(123);
        list.add(456);
        list.add("AA");

        //方式一：Iterator迭代器方式
        Iterator iterator = list.iterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        }

        System.out.println("***************");

        //方式二：增强for循环
        for(Object obj : list){
            System.out.println(obj);
        }

        System.out.println("***************");

        //方式三：普通for循环
        for(int i = 0;i < list.size();i++){
            System.out.println(list.get(i));
        }
    }

    @Test
    public void tets2(){
        ArrayList list = new ArrayList();
        list.add(123);
        list.add(456);
        list.add("AA");
        list.add(new Person("Tom",12));
        list.add(456);
        //int indexOf(Object obj):返回obj在集合中首次出现的位置。如果不存在，返回-1.
        int index = list.indexOf(4567);
        System.out.println(index);

        //int lastIndexOf(Object obj):返回obj在当前集合中末次出现的位置。如果不存在，返回-1.
        System.out.println(list.lastIndexOf(456));

        //Object remove(int index):移除指定index位置的元素，并返回此元素
        Object obj = list.remove(0);
        System.out.println(obj);
        System.out.println(list);

        //Object set(int index, Object ele):设置指定index位置的元素为ele
        list.set(1,"CC");
        System.out.println(list);

        //List subList(int fromIndex, int toIndex):返回从fromIndex到toIndex位置的左闭右开区间的子集合
        List subList = list.subList(2, 4);
        System.out.println(subList);
        System.out.println(list);
    }

    @Test
    public void test(){
        ArrayList list = new ArrayList();
        list.add(123);
        list.add(456);
        list.add("AA");
        list.add(new Person("Tom",12));
        list.add(456);

        System.out.println(list);

        //void add(int index, Object ele):在index位置插入ele元素
        list.add(1,"BB");
        System.out.println(list);

        //boolean addAll(int index, Collection eles):从index位置开始将eles中的所有元素添加进来
        List list1 = Arrays.asList(1, 2, 3);
        list.addAll(list1);
//        list.add(list1);
        System.out.println(list.size());//9

        //Object get(int index):获取指定index位置的元素
        System.out.println(list.get(2));

    }
}

```

> 关于remove()方法的面试题

```java
import org.junit.Test;

import java.util.ArrayList;
import java.util.List;

public class ListEver {
    /**
     * 区分List中remove(int index)和remove(Object obj)
     */

    @Test
    public void testListRemove() {
        List list = new ArrayList();
        list.add(1);
        list.add(2);
        list.add(3);
        updateList(list);
        System.out.println(list);//
    }

    private void updateList(List list) {
        // 此处调用的事remove(int index)的方法
//        list.remove(2);
        // 此处调用的事remove(Object obj)的方法
        list.remove(new Integer(2));
    }
}
```



#### 2.Set：无序的、不可重复的

> - Set接口中没有定义新的方法，使用的都是collection中声明过的方法
> - 要求：向Set(主要指：HashSet、LinkedHashSet)中添加的数据，其所在的类一定要重写hashCode()和equals()
> - 要求：重写的hashCode()和equals()尽可能保持一致性：相等的对象必须具有相等的散列码
> - 重写两个方法的小技巧：对象中用作 equals() 方法比较的 Field，都应该用来计算 hashCode 值。

##### 2.1 **HashSet**：作为Set接口的主要实现类，线程不安全，可以存储null值

```java
//	1.无序性：不等于随机性，存储的数据在底层数组中并非按照数组索引的顺序添加，而是根据数据的哈希值
//	2.不可重复性：保证添加的元素按照equals()方法判断时，不能返回true，即相同的元素只能添加一个
/** 
 * 二、添加元素的过程：以HashSet为例：
 *      我们向HashSet中添加元素a,首先调用元素a所在类的hashCode()方法，计算元素a的哈希值，
 *      此哈希值接着通过某种算法计算出在HashSet底层数组中的存放位置（即为：索引位置），判断
 *      数组此位置上是否已经有元素：
 *          如果此位置上没有其他元素，则元素a添加成功。 --->情况1
 *          如果此位置上有其他元素b(或以链表形式存在的多个元素），则比较元素a与元素b的hash值：
 *              如果hash值不相同，则元素a添加成功。--->情况2
 *              如果hash值相同，进而需要调用元素a所在类的equals()方法：
 *                    equals()返回true,元素a添加失败
 *                    equals()返回false,则元素a添加成功。--->情况3
 *
 *      对于添加成功的情况2和情况3而言：元素a 与已经存在指定索引位置上数据以链表的方式存储。
 *      jdk 7 :元素a放到数组中，指向原来的元素。
 *      jdk 8 :原来的元素在数组中，指向元素a
 *      总结：七上八下
 *
 * HashSet底层：数组+链表的结构。
 */
```

> IDE中对hashCode()方法的重写，为什么用31这个数字？

```java
@Override
public int hashCode() {
    final int prime = 31;
    int result = 1;
    result = prime * result + ((id == null) ? 0 : id.hashCode());
    result = prime * result + ((name == null) ? 0 : name.hashCode());
    return result;
}

/**
 * 选择系数的时候要选择尽量大的系数。因为如果计算出来的hash地址越大，所谓的“冲突”就越少，查找起来效率也会提高。（减少冲突）
 * 并且31只占用5bits,相乘造成数据溢出的概率较小。
 * 31可以由i*31== (i<<5)-1来表示,现在很多虚拟机里面都有做相关优化。（提高算法效率）
 * 31是一个素数，素数作用就是如果我用一个数字来乘以这个素数，那么最终出来的结果只能被素数本身和被乘数还有1来整除！(减少冲突)
 */
```
- **LinkedHashSet**：作为HashSet的子类，遍历其内部数据时，可以按照添加的顺序遍历

```java
/**
 * LinkedHashSet是HashSet的子类
 * LinkedHashSet根据元素的hashCode值来决定元素的存储位置，但它同时使用双向链表维护元素的次序，这使得元素看起来是以插入序保存的。
 * LinkedHashSet插入性能略低于HashSet，但在迭代访问Set 里的全部元素时有很好的性能。
 * LinkedHashSet不允许集合元素重复。
 */
```

##### 2.2 **TreeSet**：可以按照添加的对象的指定属性，进行排序

1. 向TreeSet中添加数据，要求是相同类的对象
2. 两种排序方式：自然排序（实现Comparable接口）和自定义排序（Comparator）
3. 自然排序中，比较两个对象是否有相同的标准：compareTo()返回0，不是equals().
4. 定制排序中，比较两个对象是否相同的标准为：compare()返回0.不再是equals().

#### 3.Queue

### 二、Map：双列数据，存储Key-Value对的数据

```java
/**
 *  Map中的key:无序的、不可重复的，使用Set存储所有的key  ---> 要求key所在的类要重写equals()和hashCode() （以HashMap为例）
 *  Map中的value:无序的、可重复的，使用Collection存储所有的value --->value所在的类要重写equals()
 *  一个键值对：key-value构成了一个Entry对象。
 *  Map中的entry:无序的、不可重复的，使用Set存储所有的entry
 */
```



#### 1.**HashMap**：Map的主要实现类：线程不安全的，效率高，可以存储null的key和value

> HashMap的底层：数组+链表（jdk7及以前），数组+链表+红黑树（jdk8）

```java
HashMap的底层实现原理：
JDK7：
	HashMap hashMap = new HashMap(); // 在实例化以后，底层创建了长度是16的一维数组Entry[] table。
	hashMap.put("xxx",xxx);
	...可能已经执行多次put...
    hashMap.put(key,value);
        首先，调用key所在类的hashCode()计算key1哈希值，此哈希值经过某种算法计算以后，得到在Entry数组中的存放位置。
        如果此位置上的数据为空，此时的key-value添加成功。	---情况1
        如果此位置上的数据不为空，意味着此位置上有一个或多个数据（以链表形式存在），比较key和已存在的一个或多个数据的哈希值：
            如果key的哈希值与已经存在的数据的哈希值都不相同，此时key-value添加成功。	---情况2
            如果key的哈希值与已经存在的某个数据(key1,value1)的哈希值相同，继续比较：调用key所在类的equals()方法，比较：
            	如果equals()返回false：此时key-value添加成功。	---情况3
            	如果equals()返回true：使用value替换value1。
            
    	注：关于情况2和清空3：此时的key-value和原来的数据以链表的方式存储
            
        在不断的添加过程中，会涉及到扩容问题，当超出临界值(且要存放的位置非空)时，扩容。默认的扩容方式：扩容为原来容量的2倍，并将原有的数据复制过来。

JDK8 相较于JDK7在底层的实现方面的不通：
	1.new HashMap()：底层没有创建一个长度为16的数组。
	2.JDK8底层的数组是Node[],而非Entry[]
	3.在首次调用put()方法时，底层创建长度为16的数组
	4.JDK7的底层结构只有：数组+链表。JDK8中结构：数组+链表+红黑树。
        当数组的某一个索引位置上的元素以链表的形式存在的个数 > 8 且当前数组长度 >64,此时此索引位置上的所数据改为使用红黑树存储。

            
```



- **LinkedHashMap**：保证在遍历map元素时，可以按照添加的顺序实现遍历。原因：在原有的HashMap底层结构基础上，添加了一对指针，指向前一个和后一个元素。对于频繁的遍历操作，此类执行效率优于HashMap

#### 2.**HashTable**：古老的实现类，线程安全，效率低，不能存储null的key和value

-  **Properties**：常用用来处理配置文件，key和value都是String类型

#### 3.**TreeMap**：保证按照添加的key-value对进行排序，实现排序遍历。此时考虑key的自然排序或自定义排序，底层使用红黑树

```java
// 向TreeMap中添加key-value，要求key必须是由同一个类创建的对象，因为要按照key进行排序：自然排序和自定义排序
```

