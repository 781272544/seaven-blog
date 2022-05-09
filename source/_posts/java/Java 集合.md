---
title: Java-集合
date: 2022-05-01 19:25:52
tags: ["java","Java集合"]
categories: ["java"]
---
数组特点：初始化后长度确定了，元素的类型也确定了。有序、可重复。

数组弊端：初始化后长度不可修改，添加、删除等操作效率不高。

==java中链表的实现：在Node类中定义一个Node 类型的 next对象作为类属性==

**collection接口：**单列集合。向collection接口的实现类的对象中添加数据obj时，要求obj所在类重写equals方法，contains方法不重写equals就比较地址。

​	**子接口list：**存储有序、可重复的数据。

​		**实现类ArrayList：**线程不安全，效率高；底层使用Object[] elementData存储。默认大小为：10，一次扩容1.5倍[10、15、22]。在JDK1.8中第一次调用add方法，底层才创建了长度为10的数组。类似于单例模式中的懒汉式。建议使用带参构造器。

​		**实现类LinkedList：**对于频繁的插入、删除操作，效率比ArrayList高；底层使用双向链表存储。

​		**实现类Vector：**线程安全，效率低；底层使用Object[] elementData存储。默认大小为：10，一次扩容2倍。

​	**子接口set：**存储无序、不可重复的数据。 要求：1.向set中添加的数据，其所在的类一定要重写hashCode()和equals()；2.重写的hashCode()和equals()尽可能保持一致性：相等的对象必须具有相同的散列码。

​		**实现类HashSet：**线程不安全；可以存储null值。底层使用数组+链表存储（JDK1.8 HashMap{value的值为object类型的PRESENT常量}+链表）。无序性：数据存储在底层的数组中并非按照数组索引添加，而是按照数据的哈希值。不可重复性：保证添加的元素按照equals()判断时，不能返回true。

​			HashSet添加元素过程：首先调用元素a所在类的hashCode方法，计算元素a的哈希值，根据哈希值计算出在hashSet底层数组中的存放位置（索引位置），若此位置没有元素，则元素a添加成功；若此位置有值b（或以链表的形式存在多个元素），则比较a与b的哈希值。若哈希值不同，则元素a添加成功；若哈希值相同，则调用元素a所在类的equals方法，返回值为true时，元素a添加失败；返回值为false时，元素a添加成功。

​			**子类LinkedHashset：**遍历时，可以按照添加的顺序遍历。底层使用数组+双向链表存储(JDK1.8 HashMap+链表)。优点：对于频繁的遍历操作，LinkedHashSet效率高于HashSet。

​		**实现类TreeSet：**可以按照添加对象的指定属性进行排序；底层使用红黑树存储。1.只能添加相同类的对象。2.支持自然排序（实现compareable接口）和定制排序（Comparator）；自然排序中，比较两个对象是否相同的标准是：compareTo(Object obj)返回0，不再是equals()。定制排序中，比较两个对象是否相同的标准是：compare(Object obj1,Object obj2)返回0，不再是equals()。

```java
TreeSet set = new TreeSet(new Comparator(){
    public int compare(Object o1,Object o2){
        if(o1 instanceof User && o2 instanceof User){
            User u1=(User)o1;
            User u2=(User)o2;
            return u1.age-u2.age;
        }
        throw new RuntimeException("error");
    }
});
```

```java
public int compareTo(Object o){
    if(o instanceof User){
        User u=(User)o;
        return this.name.compareTo(u.name);
    }   
    throw new RuntimeException("error");
}
```





**Map接口：**双列集合，存储一对（key-value）的数据。key：无序、不可重复，使用set存储。value：无序、可重复，使用collection存储。key-value构成了一个Entry对象，它是无序、不可重复的，使用set存储。

​	**HashMap：** 线程不安全，效率高。底层数组+链表。要求key所在类重写equals方法和hashCode方法；要求value所在类重写equals方法。

​		HashMap map = new HashMap()	在实例化以后，底层创建了一个长度为16的一维数组Entry[] table

​		map.put(key1,value1)：首先调用key1所在类的hashCode方法计算key1的哈希值，根据哈希值得到Entry数组中的存放位置。若此位置的数据为空，则key1-value1添加成功；若此位置的数据不为空（此位置上存在一个或多个数据[以链表的形式]），则比较key1与已经存在的一个或多个数据(key2-value2)的哈希值：如果key1的哈希值与已经存在的一个或多个数据(key2-value2)的哈希值都不相同，那么key1-value1添加成功；如果key1的哈希值与已经存在的一个或多个数据(key2-value2)的哈希值相同，那么继续比较：调用key1所在类的equals方法，若equals方法返回false，则key1-value1添加成功；equals方法返回true，则使用value1替换value2。

​		默认扩容方式：扩容为原来的2倍，并将原来的数据复制过来。

​		**JDK1.8：**1.new HashMap()时，底层没有创建一个长度为16的数组	2.不用Entry[]，而用Node[]数组 3.首次调用put方法时才创建数组。4.底层采用：数组+链表+红黑树。当数组的某个索引位置上的元素，以链表的形式存在的数据个数>8且当前数组的长度>64时，该索引位置上的所有数据改用红黑树存储。

​			DEFAULT_INITIAL_CAPACITY = 1 << 4;	00001左移4位就是10000，即为16。默认容量为16

​				21<<1=42	10101左移变为101010	【相当于21乘以2的一次方？】

​				21>>1=10	10101右移变为1010 		【相当于21除以2的一次方？】

​			DEFAULT_LOAD_FACTOR = 0.75f;	默认加载因子。此值比较大时，占用的空间较小，发生hash冲突的机率会变大。此值比较小时，会占用更多的空间，发生hash冲突的机率会变小。

​			threshold：扩容的临界值=容量x填充因子=16x0.75=12

​			TREEIFY_THRESHOLD = 8;		链表长度大于该默认值时，转化为红黑树

​			UNTREEIFY_THRESHOLD = 6;      红黑树长度小于该值时，转化为链表

​			MIN_TREEIFY_CAPACITY = 64; 		当把链表转换为红黑树时，Node[]数组的最小值，否则扩容Node[]数组

​		红黑树：因为一旦链表过长，会严重影响HashMap的性能，而红黑树具有快速增删改查的特点，就可以解决链表过长时操作比较慢的问题。

​		JDK1.7中多线程操作HashMap时可能引起死循环，原因是：扩容转移后，前后链表顺序倒置，在转移过程中修改了原链表中节点的引用关系

![](D:\笔记文件\picture\HashMap元素添加过程.png)

​		**LinkedHashMap：** 继承于HashMap。保证在遍历map元素时，可以按照添加的顺序实现遍历。底层在HashMap的基础上添加了前驱指针和后继指针。对于频繁的遍历操作，此类执行效率高于HashMap。

​	**TreeMap：**保证按照添加的key-value对进行排序，实现排序遍历。要求key是由同一个类创建的对象，此时考虑key的自然排序和定制排序。底层使用红黑树。

​	**Hashtable：**线程安全，效率低。不能存储null的key和value。

​		**子类Properties：**常用来处理配置文件。key-value都是String类型。



* collection ：要求向collection接口的实现类的对象中添加数据obj时，要求obj所在类重写equals方法
  * contains(Object obj)：判断当前集合中是否包含obj。在判断时会调用obj对象所在类的equals()。
  * containAll(Collection collection)：判断形参collection中的所有元素是否都存在于当前集合中
  * remove(Object obj)：从当前集合中移除obj元素
  * removeAll(Collection collection)：从当前集合中移除collection中所有元素
  * retainAll(Collection collection)：获取当前集合中和collection的交集元素
  * equals(Collection collection)：判断形参collection是否和当前集合相等
  * hashCode()：返回当前集合对象的哈希值
  * toArray()：将集合转换成数组
  * Arrays.asList()：调用Arrays类的静态方法asList，将数组转换成集合
  * 

* ArrayList：
  * add(int index,Object obj)	插入	add(Object obj)增
  * addAll(int index,Collection coll)
  * get(int index)      查
  * indexOf(Object obj)
  * lastIndexOf(Object obj)
  * remove(int index)         删       remove(Object obj)
  * set(int index,Object obj) 替换
  * subList(int fromIndex,int toIndex)

* Set中使用的是Collection中的方法

* Map：

  * put(Object key,Object value)
  * putAll(Map m) 将m中的所有键值对存放到当前map中
  * remove(Object key) 清除指定key的键值对，返回value值
  * clear() 清空当前map中的所有数据
  * get(Object key)
  * containsKey(Object key)
  * containsValue(Object value)
  * size()
  * isEmpty() 
  * equals() 比较两个键值对是否相同
  * Set keySet()  遍历map的所有key
  * Collection values()  遍历map的所有value
  * Set<Map.Entry<K,V>> entrySet()  遍历map的所有key-value

* Perperties

  * load(FileInputStream fis) 加载流对应的文件
  * getPerperty(String string) 根据指定的key获取value

* Collections用于操作collection和map工具类

  * reverse(List)	反转list中的元素的顺序

  * shuffle(List)    对list中的元素进行随机排序

  * sort(List)       根据元素的自然顺序对指定list集合元素按升序排序

  * sort(List , Comparator)     根据Comparator产生的顺序对指定list集合元素进行排序

  * swap(List , int i , int j)       将list集合中的i处元素和j处元素进行交换

  * Object max(Collection)      根据元素的自然顺序，返回集合中的最大值

  * Object max(Collection , Comparator)        根据Comparator产生的顺序，返回集合中的最大值

  * Object min(Collection)

  * Object min(Collection , Comparator)

  * int frequency(Collection , Object)         返回集合中指定元素出现的次数

  * void copy(List dest , List src)       将src中的内容复制到dest中。要求dest.size()>=src.size()

    ​	List dest = ArrayList.asList(new Object[src.size()]);

  * boolean replaceAll(List list , Object oldVal , Object newVal)         使用newVal替换集合中的oldVal

  * synchronizedXxx()     该方法可使指定集合包装成线程同步的集合，从而解决多线程并发访问集合时的线程安全问题。


```java 
//foreach循环遍历集合、数组
collection.forEach(System.out::println);
//
int a[] = new a[5];
System.out.println(Arrays.toString(a));
```

for增强循环遍历集合、数组

```java
Collection collection = new ArrayList();
collection.add(123);
collection.add("hello");
collection.add(true);
//for（集合元素类型 局部变量 ： 集合对象）
//内部调用迭代器
for(Object obj : collection)
    System.out.println(obj);
```

==遍历ArrayList时如何正确移除一个元素，禁止使用循环来删除元素，因为删除一个元素后，size变小了==

```java
public static void main(String args[]){
    List<String> list = new ArrayList();
    list.add("a");
    list.add("b");
    list.add("c");
    list.add("d");
    list.add("e");
    //每次调用iterator方法都会得到一个全新的Iterator对象
    //it.next()指向第一个元素
    Iterator<String> it = list.iterator();
    while(it.hasNext()){
        if(it.next().equals("c"))
            //使用iterator的remove方法
            it.remove();
    }
}
```

* 迭代器修改元素

  ```
  List<String> list = new ArrayList();
  ListIterator<CatalogEntity> iterator = list.listIterator();
  ```

  

* 泛型：就是允许在定义类、接口时通过一个标识表示类中某个属性的类型或者是某个方法的返回值及参数类型
  * 集合中使用泛型：1.在实例化集合类时，可以指定具体的泛型类型。2.指定了泛型后，集合中的内部结构使用到泛型的位置，都为实例化时指定的泛型。3.泛型必须是类，不能是基本类型。4.默认使用Object
  * 泛型类：静态方法中不能使用**类的泛型**，静态方法在创建对象前执行。异常类不能使用泛型。

```java
public class Order<T>{...}
public class SubOrder<T> extends Order<T>{...}//此时SubOrder为泛型类
public class SubOrder extends Order<String>{...}//此时SubOrder不是泛型类
```

* 泛型方法:可以声明为静态的，因为：泛型参数是在调用方法的时候确定的，并非在实例化时确定。

  ```java
  //方法中出现的泛型参数与类的泛型参数没有关系
  public static <E> List<E> copyFromArrayToList(E[] array){
      ArrayList<E> list = new ArrayList<>();
      for(E e : array)
          list.add(e);
      return list;
  }
  ```

* 通配符：?

  ```java
  List<Object> list1=null;
  List<String> list1=null;
  List<?> list=null;
  list=list1;
  lsit=list2;
  ```

  ? extends A ，即A的任意子类，用象限表示为：(?，A]

  ? super A，即A的任意父类，用象限表示为：[A，?)       
