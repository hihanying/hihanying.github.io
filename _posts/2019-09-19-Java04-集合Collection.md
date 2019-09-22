---
layout:     post                   
title:      Java04集合
subtitle:  Java集合是java提供的工具包，包含了常用的数据结构
date:       2019-09-19        
author:     HY                 
header-img: img/post-bg-2015.jpg
catalog: true                  
tags:                   
    - Java
    - 集合
---


# Java04：集合

## 1 Collection

###  1.1 Collection概述

#### 简介

- Java集合是java提供的工具包，包含了常用的数据结构：集合、链表、队列、栈、数组、映射等。
- Java集合工具包位置是`java.util.*`
- Java集合主要可以划分为4个部分：List列表、Set集合、Map映射、工具类(Iterator迭代器、Enumeration枚举类、Arrays和Collections)。

#### 框架

Java集合工具包框架图：
![](https://raw.githubusercontent.com/hihanying/FigureBed/master/img/20190921095204.png)

1. `Collection`是一个接口，是高度抽象出来的集合，它包含了集合的基本操作和属性。`Collection`包含了`List`和`Set`两大分支。
    - `List`是一个**有序的队列**，每一个元素都有它的索引。第一个元素的索引值是0。List的实现类有`LinkedList`, `ArrayList`,` Vector`, `Stack`。
    - `Set`是一个**不允许有重复元素**的集合。Set的实现类有`HastSet`和`TreeSet`。`HashSet`依赖于`HashMap`，它实际上是通过`HashMap`实现的；`TreeSet`依赖于`TreeMap`，它实际上是通过`TreeMap`实现的。
2. `Map`是一个映射接口，即**`key-value`键值对**。`Map`中的每一个元素包含一个`key`和`key`对应的`value`。
    - `AbstractMap`是个抽象类，它实现了`Map`接口中的大部分API。而`HashMap`，`TreeMap`，`WeakHashMap`都是继承于`AbstractMap`。
    - `Hashtable`虽然继承于`Dictionary`，但它实现了`Map`接口。
3. 工具类
    - `Iterator`是遍历集合的工具，即我们通常通过`Iterator`迭代器来遍历集合。我们说`Collection`依赖于`Iterator`，是因为`Collection`的实现类都要实现`iterator()`函数，返回一个`Iterator`对象。`ListIterator`是专门为遍历`List`而存在的。
    - `Enumeration`，它是JDK 1.0引入的抽象类。作用和`Iterator`一样，也是遍历集合；但是`Enumeration`的功能要比`Iterator`少。在上面的框图中，`Enumeration`只能在`Hashtable`, `Vector`, `Stack`中使用。
    - `Arrays`和`Collections`是操作数组、集合的两个工具类。

#### 设计特点

1. 实现了接口和实现类相分离，例如，有序表的接口是`List`，具体的实现类有`ArrayList`，`LinkedList`等
2. 支持泛型，我们可以限制在一个集合中只能放入同一种数据类型的元素，例如：
```java
List<String> list = new ArrayList<>(); // 只能放入String类型
```
最后，Java访问集合总是通过统一的方式——**迭代器**（`Iterator`）来实现/遍历，它最明显的好处在于无需知道集合内部元素是按什么方式存储的。

### 1.2 Collection常用功能

- `public boolean add(E e)`:   把给定的对象添加到当前集合中 。
- `public void clear()` :清空集合中所有的元素。
- `public boolean remove(E e)`: 把给定的对象在当前集合中删除。
- `public boolean contains(E e)`: 判断当前集合中是否包含给定的对象。
- `public boolean isEmpty()`: 判断当前集合是否为空。
- `public int size()`: 返回集合中元素的个数。
- `public Object[] toArray()`: 把集合中的元素，存储到数组中。

### 1.3 Iterator迭代器

`Iterator`主要用于迭代访问（即遍历）`Collection`中的元素，因此`Iterator`对象也被称为迭代器。

#### 使用Iterator迭代器

（集合中）获取迭代器的方法：
- `public Iterator iterator()`: 获取集合对应的迭代器，用来遍历集合中的元素的。

Iterator接口的常用方法如下：
- `public E next()`:返回迭代的下一个元素。
- `public boolean hasNext()`:如果仍有元素可以迭代，则返回 true。

使用Iterator迭代集合中元素：
```java
public class IteratorDemo {
  	public static void main(String[] args) {
        // 使用多态方式 创建对象
        Collection<String> coll = new ArrayList<String>();
        // 添加元素到集合
        coll.add("串串星人");
        coll.add("吐槽星人");
        coll.add("汪星人");
        //遍历（每个集合对象都有自己的迭代器）
        Iterator<String> it = coll.iterator();
        // 泛型指的是迭代出元素的数据类型
        while(it.hasNext()){ //判断是否有迭代元素
            String s = it.next();//获取迭代出的元素
            System.out.println(s);
        }
  	}
}
```
#### for each

增强for循环(也称for each循环)是**JDK1.5**以后出来的一个高级for循环，专门用来遍历数组和集合的。它的内部原理其实是个Iterator迭代器，所以在遍历的过程中，不能对集合中的元素进行增删操作。

格式：
```java
for(元素的数据类型  变量 : Collection集合or数组){ 
    //操作代码
}
```

## 2 泛型

### 2.1 概述

Collection虽然可以存储各种对象，但实际上通常Collection只存储同一类型对象，导致取出时强转引发运行时 `ClassCastException`。

在JDK5之后，新增了**泛型**(**Generic**)语法，让你在设计API时可以指定类或方法支持泛型，这样我们使用API的时候也变得更为简洁，并得到了**编译时期的语法检查**。

**泛型**可以在类或方法中预支地使用未知的类型。一般在创建对象时，将未知的类型确定具体的类型。当没有指定泛型时，默认类型为`Object`类型。

### 2.2  泛型的定义与使用

#### 定义和使用含有泛型的类

定义格式：

```
修饰符 class 类名<代表泛型的变量> {  }
```

例如：

```java
class ArrayList<E>{ 
    public boolean add(E e){ }//参数列表
    public E get(int index){ }//返回值
   	....
}
```

使用泛型：在**创建对象的时候**确定泛型。 

例如，`ArrayList<String> list = new ArrayList<String>();`

自定义泛型类
```java
public class MyGenericClass<T> {
	//没有MVP类型，在这里代表 未知的一种数据类型 未来传递什么就是什么类型
	private T mvp;
    public void setMVP(T mvp) {
        this.mvp = mvp;
    }
    public T getMVP() {
        return mvp;
    }
}
```

#### 含有泛型的方法
定义格式：

```
修饰符 <代表泛型的变量> 返回值类型 方法名(参数){  }
```

例如，

```java
public class MyGenericMethod {	  
    public <MVP> void show(MVP mvp) {
    	System.out.println(mvp.getClass());
    }
    
    public <MVP> MVP show2(MVP mvp) {	
    	return mvp;
    }
}
```

使用格式：**调用方法时，确定泛型的类型**

```java
public class GenericMethodDemo {
    public static void main(String[] args) {
        // 创建对象
        MyGenericMethod mm = new MyGenericMethod();
        // 演示看方法提示
        mm.show("aaa");
        mm.show(123);
        mm.show(12.45);
    }
}
```

#### 含有泛型的接口
**定义格式：**

```
修饰符 interface接口名<代表泛型的变量> {  }
```

例如，

```java
public interface MyGenericInterface<E>{
	public abstract void add(E e);
	
	public abstract E getE();  
}
```

**使用格式：**

实现类：

```java
public class MyImp2<E> implements MyGenericInterface<E> {
	@Override
	public void add(E e) {
       	 // 省略...
	}

	@Override
	public E getE() {
		return null;
	}
}
```

确定泛型：

```java
public class GenericInterface {
    public static void main(String[] args) {
        MyImp2<String>  my = new MyImp2<String>();  
        my.add("aa");
    }
}
```
### 2.3 泛型通配符
当使用泛型类或者接口时，传递的数据中，泛型类型不确定，可以通过通配符`<?>`表示。但是一旦使用泛型的通配符后，只能使用Object类中的共性方法，集合中元素自身方法无法使用。

#### 通配符基本使用

泛型的通配符:**不知道使用什么类型来接收的时候,此时可以使用`?`, `?`表示未知通配符。**

此时只能接受数据,不能往该集合中存储数据。


```java
public static void main(String[] args) {
    Collection<Intger> list1 = new ArrayList<Integer>();
    getElement(list1);
    Collection<String> list2 = new ArrayList<String>();
    getElement(list2);
}
public static void getElement(Collection<?> coll){}
//？代表可以接收任意类型
```

> 注意:
> 泛型不存在继承关系 `Collection<Object> list = new ArrayList<String>();`这种是错误的。

#### 通配符高级使用----受限泛型

在JAVA的泛型中可以指定一个泛型的**上限**和**下限**。

**泛型的上限**：

- **格式**： `类型名称 <? extends 类 > 对象名称`
- **意义**： `只能接收该类型及其子类`

**泛型的下限**：

- **格式**： `类型名称 <? super 类 > 对象名称`
- **意义**： `只能接收该类型及其父类型`

比如：现已知Object类，String 类，Number类，Integer类，其中Number是Integer的父类

```java
public static void main(String[] args) {
    Collection<Integer> list1 = new ArrayList<Integer>();
    Collection<String> list2 = new ArrayList<String>();
    Collection<Number> list3 = new ArrayList<Number>();
    Collection<Object> list4 = new ArrayList<Object>();
    
    getElement(list1);
    getElement(list2);//报错
    getElement(list3);
    getElement(list4);//报错
  
    getElement2(list1);//报错
    getElement2(list2);//报错
    getElement2(list3);
    getElement2(list4);
  
}
// 泛型的上限：此时的泛型?，必须是Number类型或者Number类型的子类
public static void getElement1(Collection<? extends Number> coll){}
// 泛型的下限：此时的泛型?，必须是Number类型或者Number类型的父类
public static void getElement2(Collection<? super Number> coll){}
```

## 3 List

### 3.1 List简述

#### List框架图

![](https://raw.githubusercontent.com/hihanying/FigureBed/master/img/20190921112334.png)

1. `List `是一个接口，它继承于Collection的接口。它代表着有序的队列。
2. `AbstractList` 是一个抽象类，它继承于AbstractCollection。AbstractList实现List接口中除size()、get(int location)之外的函数。
3. `AbstractSequentialList` 是一个抽象类，它继承于AbstractList。AbstractSequentialList 实现了“链表中，根据index索引值操作链表的全部函数”。
4. `ArrayList`, `LinkedList`, `Vector`,` Stack`是List的4个实现类。
   - `ArrayList` 是一个**数组队列，相当于动态数组**。它由数组实现，随机访问效率高，随机插入、随机删除效率低。
   - `LinkedList `是一个**双向链表**。它也可以被当作堆栈、队列或双端队列进行操作。LinkedList随机访问效率低，但随机插入、随机删除效率高。
   - `Vector`是**矢量队列，和ArrayList一样，它也是一个动态数组，由数组实现**。但是ArrayList是非线程安全的，而Vector是线程安全的。
   - `Stack` 是**栈，它继承于Vector**。它的特性是：先进后出(FILO, First In Last Out)。

#### List特点

1. 它是一个元素存取有序的集合。
2. 它是一个带有索引的集合，通过索引就可以精确的操作集合中的元素
3. `list`中可以有重复的元素，通过元素的equals方法，来比较是否为重复的元素。

### 3.2 List接口中的常用方法

- `public void add(int index, E element)`: 将指定的元素，添加到该集合中的指定位置上。
- `public E get(int index)`:返回集合中指定位置的元素。
- `public E remove(int index)`: 移除列表中指定位置的元素, 返回的是被移除的元素。
- `public E set(int index, E element)`:用指定元素替换集合中指定位置的元素,返回值的更新前的元素。

### 3.3 List的子类

#### ArrayList集合

`java.util.ArrayList`是一个**数组队列**，相当于 **动态数组**。与Java中的数组相比，它的容量能动态增长。

**ArrayList的继承与实现**

```java
public class ArrayList<E>extends AbstractList<E>implements List<E>, RandomAccess, Cloneable, Serializable
```

- ArrayList 继承了`AbstractList`，实现了List。它是一个数组队列，提供了相关的添加、删除、修改、遍历等功能。
- ArrayList 实现了`RandmoAccess`接口，即提供了随机访问功能。RandmoAccess是java中用来被List实现，为List提供快速访问功能的。在ArrayList中，我们即可以通过元素的序号快速获取元素对象；这就是快速随机访问。
- ArrayList 实现了`Cloneable`接口，即覆盖了函数clone()，能被克隆。
- ArrayList 实现`java.io.Serializable`接口，这意味着ArrayList支持序列化，能通过序列化去传输。

> Notes
>
> - ArrayList 实际上是**通过一个数组去保存数据的**。当我们构造ArrayList时；若使用默认构造函数，则ArrayList的**默认容量大小是10**。
> - 当ArrayList容量不足以容纳全部元素时，ArrayList会重新设置容量：**新的容量=“(原始容量x3)/2 + 1”**。
> - ArrayList的克隆函数，即是将全部元素克隆到一个数组中。
> - ArrayList实现`java.io.Serializable`的方式。当写入到输出流时，先写入“容量”，再依次写入“每一个元素”；当读出输入流时，先读取“容量”，再依次读取“每一个元素”。

 

**ArrayList支持3种遍历方式**

1. **通过迭代器遍历**。
```java
Integer value = null;
Iterator iter = list.iterator();
while (iter.hasNext()) {
    value = (Integer)iter.next();
}
```

2. **随机访问，通过索引值去遍历。**
由于ArrayList实现了RandomAccess接口，它支持通过索引值去随机访问元素。
```java
Integer value = null;
int size = list.size();
for (int i=0; i<size; i++) {
    value = (Integer)list.get(i);        
}
```

3. **for循环遍历**。
```java
Integer value = null;
for (Integer integ:list) {
    value = integ;
}
```



#### LinkedList集合

**LinkedList简介**

`java.util.LinkedList`集合数据存储的结构是链表结构。方便元素添加、删除的集合。

```java
public class LinkedList<E>extends AbstractSequentialList<E>implements List<E>, Deque<E>, Cloneable, Serializable
```

- LinkedList 是一个继承于AbstractSequentialList的双向链表。它也可以被当作堆栈、队列或双端队列进行操作。
- LinkedList 实现` List `接口，能对它进行队列操作。
- LinkedList 实现` Deque` 接口，即能将`LinkedList`当作双端队列使用。
- LinkedList 实现了`Cloneable`接口，即覆盖了函数`clone()`，能克隆。
- LinkedList 实现`java.io.Serializable`接口，这意味着`LinkedList`支持序列化，能通过序列化去传输。
- LinkedList 是非同步的。
- 

**LinkedList常用方法**

- `public void addFirst(E e)`:将指定元素插入此列表的开头。
- `public void addLast(E e)`:将指定元素添加到此列表的结尾。
- `public E getFirst()`:返回此列表的第一个元素。
- `public E getLast()`:返回此列表的最后一个元素。
- `public E removeFirst()`:移除并返回此列表的第一个元素。
- `public E removeLast()`:移除并返回此列表的最后一个元素。
- `public E pop()`:从此列表所表示的堆栈处弹出一个元素。
- `public void push(E e)`:将元素推入此列表所表示的堆栈。
- `public boolean isEmpty()`：如果列表不包含元素，则返回true。



**LinkedList的本质是双向链表**

1. LinkedList继承于AbstractSequentialList，并且实现了Dequeue接口。
2. LinkedList包含两个重要的成员：header 和 size。
   - `header`是双向链表的表头，它是双向链表节点所对应的类Entry的实例。Entry中包含成员变量： previous, next, element。其中，previous是该节点的上一个节点，next是该节点的下一个节点，element是该节点所包含的值。
   - `size`是双向链表中节点的个数。



> **总结**
>
> 1. 从`LinkedList`的实现方式中可以发现，它不存在`LinkedList`容量不足的问题。
> 2. `LinkedList`的克隆函数，即是将全部元素克隆到一个新的`LinkedList`对象中。
> 3. `LinkedList`实现`java.io.Serializable`。当写入到输出流时，先写入“容量”，再依次写入“每一个节点保护的值”；当读出输入流时，先读取“容量”，再依次读取“每一个元素”。由于`LinkedList`实现了`Deque`，而`Deque`接口定义了在双端队列两端访问元素的方法。提供插入、移除和检查元素的方法。每种方法都存在两种形式：一种形式在操作失败时抛出异常，另一种形式返回一个特殊值（null 或 false，具体取决于操作）。



## 4 Map

### 4.1Map架构

![](https://raw.githubusercontent.com/hihanying/FigureBed/master/img/20190921162558.png)



1. Map 是映射接口，Map中存储的内容是键值对(key-value)。
1. AbstractMap 是继承于Map的抽象类，它实现了Map中的大部分API。其它Map的实现类可以通过继承AbstractMap来减少重复编码。
1. SortedMap 是继承于Map的接口。SortedMap中的内容是排序的键值对，排序的方法是通过比较器(Comparator)。
1. NavigableMap 是继承于SortedMap的接口。相比于SortedMap，NavigableMap有一系列的导航方法；如"获取大于/等于某对象的键值对"、“获取小于/等于某对象的键值对”等等。
1. TreeMap 继承于AbstractMap，且实现了NavigableMap接口；因此，TreeMap中的内容是“有序的键值对”！
1. HashMap 继承于AbstractMap，但没实现NavigableMap接口；因此，HashMap的内容是“键值对，但不保证次序”！
1. Hashtable 虽然不是继承于AbstractMap，但它继承于Dictionary(Dictionary也是键值对的接口)，而且也实现Map接口；因此，Hashtable的内容也是“键值对，也不保证次序”。但和HashMap相比，Hashtable是线程安全的，而且它支持通过Enumeration去遍历。
1. WeakHashMap 继承于AbstractMap。它和HashMap的键类型不同，WeakHashMap的键是“弱键”。

### 4.2 Map简述

Map的定义如下：

```
public interface Map<K,V> { }
```

1. Map 是一个键值对(key-value)映射接口。**Map映射中不能包含重复的键；每个键最多只能映射到一个值**。

2. Map 接口提供三种collection 视图，允许以**键集**、**值集**或**键-值映**射关系集的形式查看某个映射的内容。

3. Map 映射顺序。有些实现类，可以明确保证其顺序，如 TreeMap；另一些映射实现则不保证顺序，如 HashMap 类。

4. Map 的实现类应该提供2个“标准的”构造方法：

   1. **void（无参数）构造方法，用于创建空映射**；
   2. **带有单个 Map 类型参数的构造方法，用于创建一个与其参数具有相同键-值映射关系的新映射。**

   实际上，后一个构造方法允许用户复制任意映射，生成所需类的一个等价映射。尽管无法强制执行此建议（因为接口不能包含构造方法），但是 JDK 中所有通用的映射实现都遵从它。

### 4.3 Map的常用API

- `public V put(K key, V value)`:  把指定的键与指定的值添加到Map集合中。
- `public V remove(Object key)`: 把指定的键 所对应的键值对元素 在Map集合中删除，返回被删除元素的值。
- `public V get(Object key)` 根据指定的键，在Map集合中获取对应的值。
- `boolean containsKey(Object key)  ` 判断集合中是否包含指定的键。
- `public Set<K> keySet()`: 获取Map集合中所有的**键**，存储到Set集合中。
- `public Set<Map.Entry<K,V>> entrySet()`: 获取到Map集合中所有的**键值**对对象的集合(Set集合)。
- `public Collection<V> values() `: 返回此映射中包含的**值**的 Collection 视图。 



**一般遍历方式**

```java
HashMap<String, String> map = new HashMap<String,String>();
// ......添加元素到集合
// 1. 获取所有的键,使用keySet方法
Set<String> keys = map.keySet();
// 2. 遍历键集,并根据键得到每一个值
for (String key : keys) {
    String value = map.get(key);
    System.out.println(key+"："+value);
```



**Map.Entry**

Map.Entry是Map中内部的一个接口，Map.Entry是**键值对**

Map集合中提供了获取所有Entry对象的方法：

- `public Set<Map.Entry<K,V>> entrySet()`: 获取到Map集合中所有的键值对对象的集合(Set集合)。

Map.Entry提供了获取对应键和对应值得方法：

- `public K getKey()`：获取Entry对象中的键。
- `public V getValue()`：获取Entry对象中的值。

**Map.Entry遍历方式**

```java
// 获取所有Entry对象
Set<Entry<String,String>> entrySet = map.entrySet();
// 遍历得到每一个entry对象的键和值
for (Entry<String, String> entry : entrySet) {
    System.out.println(entry.getKey()+":"+entry.getValue());
}
```



### 4.4 抽象类与接口

#### **AbstractMap**

AbstractMap的定义如下：

```java
public abstract class AbstractMap<K,V> implements Map<K,V> {}
```

AbstractMap类提供 Map 接口的主要实现，以最大限度地减少实现此接口所需的工作。

- 要实现不可修改的映射，编程人员只需扩展此类并提供`entrySet` 方法的实现即可。通常`entrySet `返回的` set `将依次在` AbstractSet `上实现。 返回的` set `不支持 `add()` 或 `remove() `方法，其迭代器也不支持 `remove()` 方法。
- 要实现可修改的映射，编程人员必须另外重写此类的 `put` 方法（否则将抛出 `UnsupportedOperationException`），`entrySet().iterator() `返回的迭代器也必须另外实现其`remove`方法。

#### **SortedMap**

SortedMap的定义如下：

```java
public interface SortedMap<K,V> extends Map<K,V> { }
```

SortedMap是一个继承于Map接口的接口，它是一个**有序**的`SortedMap`键值映射。

SortedMap的排序方式有两种：**自然排序** 或者 **用户指定比较器**。 插入有序 SortedMap 的所有元素都必须实现 Comparable 接口（或者被指定的比较器所接受）。

另外，所有SortedMap 实现类都应该提供 4 个**标准**构造方法：

1. **void（无参数）构造方法**，它创建一个空的有序映射，按照键的自然顺序进行排序。
2. **带有一个 Comparator 类型参数的构造方法**，它创建一个空的有序映射，根据指定的比较器进行排序。
3. **带有一个 Map 类型参数的构造方法**，它创建一个新的有序映射，其键-值映射关系与参数相同，按照键的自然顺序进行排序。
4. **带有一个 SortedMap 类型参数的构造方法**，它创建一个新的有序映射，其键-值映射关系和排序方法与输入的有序映射相同。无法保证强制实施此建议，因为接口不能包含构造方法。

#### **NavigableMap**

NavigableMap的定义如下：

```java
public interface NavigableMap<K,V> extends SortedMap<K,V> { }
```

NavigableMap是继承于SortedMap的接口。它是一个可导航的键-值对集合，具有了为给定搜索目标报告最接近匹配项的导航方法。
NavigableMap分别提供了获取“键”、“键-值对”、“键集”、“键-值对集”的相关方法。

NavigableMap除了继承SortedMap的特性外，它的提供的功能可以分为4类：

1. **提供操作键-值对的方法。**

   -  lowerEntry、floorEntry、ceilingEntry 和 higherEntry 方法，它们分别返回与小于、小于等于、大于等于、大于给定键的键关联的 Map.Entry 对象。
   - firstEntry、pollFirstEntry、lastEntry 和 pollLastEntry 方法，它们返回和/或移除最小和最大的映射关系（如果存在），否则返回 null。

2. **提供操作键的方法**。这个和第1类比较类似
                  lowerKey、floorKey、ceilingKey 和 higherKey 方法，它们分别返回与小于、小于等于、大于等于、大于给定键的键。

3. **获取键集。**
                 navigableKeySet、descendingKeySet分别获取正序/反序的键集。

4. 获取键-值对的子集

   

## 5 Set

![](https://raw.githubusercontent.com/hihanying/FigureBed/master/img/20190921094741.png)

1. `Set` 是继承于`Collection`的接口。它是一个不允许有重复元素的集合。
2. `AbstractSet` 是一个抽象类，它继承于`AbstractCollection`，`AbstractCollection`实现了`Set`中的绝大部分函数，为`Set`的实现类提供了便利。
3. `HastSet` 和 `TreeSet` 是`Set`的两个实现类。
    - `HashSet`依赖于`HashMap`，它实际上是通过`HashMap`实现的。`HashSet`中的元素是无序的。
    - `TreeSet`依赖于`TreeMap`，它实际上是通过`TreeMap`实现的。`TreeSet`中的元素是有序的

### 5.1 HashSet

```java
public class HashSet<E>extends AbstractSet<E>implements Set<E>, Cloneable, Serializable
```
#### HashSet的特点
`java.util.HashSet`是`Set`接口的一个实现类，具有以下特点：
1. 所存储的元素是**不可重复**的，并且元素都是**无序**的(即存取顺序不一致)。
2. **没有索引**， 不能使用普通`for`循环，但 可以使用迭代器、增强`for`循环遍历。
3. 底层实际上是一个**哈希表**（HashMap），允许使用`null`元素
4. HashSet是**非同步的**。如果多个线程同时访问一个HashSet，而其中至少一个线程修改了该 set，那么它必须保持外部同步。这通常是通过对自然封装该 set 的对象执行同步操作来完成的。如果不存在这样的对象，则应该使用` Collections.synchronizedSet` 方法来“包装” set。最好在创建时完成这一操作，以防止对该 set 进行意外的不同步访问：
`Set s = Collections.synchronizedSet(new HashSet(...));`

#### HashSet的主要API

```java
boolean         add(E object)
void            clear()
Object          clone()
boolean         contains(Object object)
boolean         isEmpty()
Iterator<E>     iterator()//HashSet通过iterator()返回的迭代器是fail-fast的。
boolean         remove(Object object)
int             size()
```

`HashSet`是根据对象的哈希值来确定元素在集合中的存储位置，因此具有良好的存取和查找性能。保证元素唯一性的方式依赖于：`hashCode`与`equals`方法。因此，使用HashSet存储自定义类的对象需要重写`hashCode`与`equals`方法

#### **HashSet遍历方式**

**通过Iterator遍历HashSet**

第一步：**根据iterator()获取HashSet的迭代器。**
第二步：**遍历迭代器获取各个元素**。

```java
// 假设set是HashSet对象
for(Iterator iterator = set.iterator();
       iterator.hasNext(); ) { 
    iterator.next();
}   
```

 

**通过for-each遍历HashSet**

第一步：**根据toArray()获取HashSet的元素集合对应的数组。**
第二步：**遍历数组，获取各个元素。**

```java
// 假设set是HashSet对象，并且set中元素是String类型
String[] arr = (String[])set.toArray(new String[0]);
for (String str:arr)
    System.out.printf("for each : %s\n", str);
```



### 5.2 TreeSet

#### TreeSet简述

`TreeSet `是一个有序的集合，它的作用是提供有序的`Set`集合。它继承于`AbstractSet`抽象类，实现了`NavigableSet<E>, Cloneable, java.io.Serializable`接口。

```java
public class TreeSet<E>extends AbstractSet<E>implements NavigableSet<E>, Cloneable, Serializable
```

1. TreeSet 继承于AbstractSet，所以它是一个Set集合，具有Set的属性和方法。
2. TreeSet 实现了NavigableSet接口，意味着它支持一系列的导航方法。比如查找与指定目标最匹配项。
3. TreeSet 实现了Cloneable接口，意味着它能被克隆。
4. TreeSet 实现了java.io.Serializable接口，意味着它支持序列化

`TreeSet`是基于`TreeMap`实现的。`TreeSet`中的元素支持2种排序方式：**自然排序** 或者 **根据创建`TreeSet` 时提供的` Comparator `进行排序**。这取决于使用的构造方法。
`TreeSet`为基本操作（add、remove 和 contains）提供受保证的` log(n) `时间开销。
`TreeSet`是非同步的。 它的iterator 方法返回的迭代器是`fail-fast`的。

`TreeSet`是非线程安全的。

#### TreeSetAPI

```java
boolean                   add(E object)
void                      clear()
Object                    clone()
boolean                   contains(Object object)
boolean                   isEmpty()
boolean                   remove(Object object)
int                       size()
Iterator<E>               iterator()
```

1. TreeSet是有序的Set集合，因此支持add、remove、get等方法。

2. 和NavigableSet一样，TreeSet的导航方法大致可以区分为两类，一类时提供元素项的导航方法，返回某个元素；另一类时提供集合的导航方法，返回某个集合。
   lower、floor、ceiling 和 higher 分别返回小于、小于等于、大于等于、大于给定元素的元素，如果不存在这样的元素，则返回 null。

   
























