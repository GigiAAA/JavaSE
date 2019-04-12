## 类集---List接口与Set接口

>Collection接口
>
>List接口
>
>List具体实现三个常用子类(ArrayList、Vector、LinkedList)
>
>集合输出

### 1.Collection接口

java类集所有类均在java.util包下。

**java类集本质上是动态数组(当元素个数达到最大值时，动态增加容量)**，在JDK1.2引出。作用为解决数组定长问题。

java集合类框架是加上就是java针对于数据结构的一种实现。

在细谈List接口和Set接口之前我们先来看一下源代码：

```java
//List接口
public interface List<E> extends Collection<E> {}
//Set接口
public interface Set<E> extends Collection<E> {}
```

通过源码可以发现，List接口与Set接口均继承了Collection接口。那么我们先来研究Collection接口：

```java
public interface Collection<E> extends Iterable<E> {}
//迭代器接口
public interface Iterable<T> {
    Iterator<T> iterator();
}
```

我们通过源码可以看到，Collection接口继承了Iterable接口，但需要注意的是Iterable接口是迭代器接口，继承该类的类标志着才可以进行遍历(如for、foreach)。

**Collection接口为单个集合保存的最大接口，该接口继承了迭代器接口，表明Collection子类支持迭代。**

从JDK1.5开始Collection接口追加了泛型应用，这样的直接好处是可以避免ClassCastException，里面保存的所有数据类型应该是相同的。在JDK1.5之前Iterable接口中的iterable()方法是直接定义在Collection接口中的，之后抽象为单独接口。

接下来看一下Collection接口中比较重要的几个方法：

```java
1.boolean add(E e);//向集合中添加数据
2.boolean addAll(Collection<? extends E> c);//一次性可向集合中添加一组数据
3.void clear();//将集合中的数据清空
4.boolean contains(Object o);//查看该集合中是否存在某元素，需要equals()方法
5.boolean remove(Object o);//将集合中某一元素移除，需要equals()方法
6.boolean equals(Object o);//判断地址是否相等
7.int size();//返回集合大小
8.Object[] toArray();//将集合中元素转变为数组
9.Iterator<E> iterator();//取得集合的迭代器，用于元素输出
```

以上几个方法中尤其用到最多的是add()、iterator()方法。Collection接口为我们定义了存储数据的标准，但是无法区分存储类型。所以在实际开发中并不是直接使用Collection接口，**而是使用它的子接口List(允许数据重复)、Set(不允许数据重复)。**

Collection接口继承结构：

![类集Collection源码剖析](C:\Users\14665\source\JAVA学习\学习总结类集---List接口与Set接口\类集Collection源码剖析.jpg)

### 2.List接口

List接口是有序集合，允许元素重复，在进行单个集合处理时优先考虑List接口。

#### 首先先来学习一下List接口扩展的(List接口独有)的两个方法：

```java
1.E set(int index, E element);//修改集合中下标为index处元素值且返回修改前数值
2.E get(int index);//获取集合中下标为index的数值
```

List子接口与Collection接口相比，最大的特点是扩展了get()方法，可以根据索引下标获取集合中元素。但List还是接口，故具体操作需要依靠其子类。**List常用的三个子类为ArrayList、Vector、LinkedList。**

List接口继承关系架构：

![1](C:\Users\14665\source\JAVA学习\学习总结类集---List接口与Set接口\1.png)

```java
public abstract class AbstractList<E> extends AbstractCollection<E> implements List<E> {}
```

如上图所示，AbstractList抽象类存在的意义为将ArrayList、Vector、LinkedList三个子类共有的方法提取至AbstractList抽象类中实现，特有方法再由子类具体实现。

#### 2.1ArrayList类(常用，优先考虑)

该类从名称上可以知道其底层具体实现依靠数组。

我们先来学习一下ArrayList类的构造方法：

```java
1.无参构造(调用无参构造后并不是直接开辟空间，而是先将其置为空，在第一次添加元素时将数组开辟长度为10(默认初始长度))
public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
2.有参构造(可在调用有参构造时自行设定数组长度，但一般建议若要自行设定就大于10，否则建议使用无参构造。因若自行设定小于10的数组长度，就可能增加扩容次数，降低效率)
public ArrayList(int initialCapacity) {//调用有参构造时自行设定数组初始长度
        if (initialCapacity > 0) {//若大于0，开辟数组长度
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {//若等于0，将数组置为空
            this.elementData = EMPTY_ELEMENTDATA;
        } else {//小于0时非法抛出异常
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
    }
private static final Object[] EMPTY_ELEMENTDATA = {};
3.有参构造接收Collection对象
public ArrayList(Collection<? extends E> c) {
        elementData = c.toArray();//将集合转变为数组
        if ((size = elementData.length) != 0) {
            // c.toArray might (incorrectly) not return Object[] (see 6260652)
            if (elementData.getClass() != Object[].class)
                elementData = Arrays.copyOf(elementData, size, Object[].class);
        } else {
            // replace with empty array.
            this.elementData = EMPTY_ELEMENTDATA;
        }
    }
```

ArrayList简单实现自定义类对象保存：

```java
import java.util.ArrayList;
import java.util.List;

class Person{
    private String name;
    private Integer age;

    public Person(String name, Integer age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
public class Test{
    public static void main(String[] args) {
        List<Person> list=new ArrayList<>();
        list.add(new Person("lala",12));
        list.add(new Person("kk",19));
        list.add(new Person("lala",11));
        list.add(new Person(null,9));
        list.add(new Person(null,null));
        System.out.println(list);
        System.out.println("数组长度为："+list.size());
        System.out.println("修改元素值："+list.set(3,new Person("张三",99)));
        System.out.println("取得元素值："+list.get(3));
    }
}
//[Person{name='lala', age=12}, Person{name='kk', age=19}, Person{name='lala', age=11}, Person{name='null', age=9}, Person{name='null', age=null}]
//数组长度为：5
//修改元素值：Person{name='null', age=9}
//取得元素值：Person{name='张三', age=99}
//通过以上输出结果表明ArrayList为有序集合，先进先出，且可接收属性为null
```

#### 那么若想移除元素或者判断某元素是否存在时以上代码还可以直接使用吗？

```java
//在以上代码中直接调用remove()方法及contains()方法
import java.util.ArrayList;
import java.util.List;

class Person{
    private String name;
    private Integer age;

    public Person(String name, Integer age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
public class Test{
    public static void main(String[] args) {
        List<Person> list=new ArrayList<>();
        list.add(new Person("lala",12));
        list.add(new Person("kk",19));
        list.add(new Person("lala",11));
        list.add(new Person(null,9));
        list.add(new Person(null,null));
        System.out.println("移除元素："+list.remove(new Person("lala",12)));
        System.out.println("判断该元素是否存在："+list.contains(new Person("kk",19)));
    }
}
//移除元素：false
//判断该元素是否存在：false
//由以上输出结果我们可以发现调用contains()及remove()方法均失败，这是为什么呢？我们可以发现在list添加元素时直接使用匿名数组，那么在移除或者判断时也这样显然会失败，原因是我们都知道一旦有new就说明在堆上开辟了新空间，地址不同当然会操作失败，所以在此种情况下要使用移除判断元素是否存在就必须在自定义类中覆写equals()方法

import java.util.ArrayList;
import java.util.List;

class Person{
    private String name;
    private Integer age;

    public Person(String name, Integer age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
    @Override
    public boolean equals(Object obj) {
        if(obj==this){
            return true;
        }else if(obj==null||!(obj instanceof Person)){
            return false;
        }
        Person per=(Person) obj;
        return per.name.equals(this.name)&&per.age.equals(this.age);
    }
}
public class Test{
    public static void main(String[] args) {
        List<Person> list=new ArrayList<>();
        list.add(new Person("lala",12));
        list.add(new Person("kk",19));
        list.add(new Person("lala",11));
        list.add(new Person(null,9));
        list.add(new Person(null,null));
        System.out.println("当前数组长度为："+list.size());
        System.out.println("移除元素："+list.remove(new Person("lala",12)));
        System.out.println("判断该元素是否存在："+list.contains(new Person("kk",19)));
        System.out.println("当前数组长度为："+list.size());
    }
}
//当前数组长度为：5
//移除元素：true
//判断该元素是否存在：true
//当前数组长度为：4
```

#### 2.2Vector类(JDK1.0 使用较少)---使用上与ArrayList一致

```java
import java.util.List;
import java.util.Vector;

class Person{
    private String name;
    private Integer age;

    public Person(String name, Integer age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
    @Override
    public boolean equals(Object obj) {
        if(obj==this){
            return true;
        }else if(obj==null||!(obj instanceof Person)){
            return false;
        }
        Person per=(Person) obj;
        return per.name.equals(this.name)&&per.age.equals(this.age);
    }
}
public class Test{
    public static void main(String[] args) {
        List<Person> list=new Vector<>();
        list.add(new Person("lala",12));
        list.add(new Person("kk",19));
        list.add(new Person("lala",11));
        list.add(new Person(null,9));
        list.add(new Person(null,null));
        System.out.println("当前数组长度为："+list.size());
        System.out.println("移除元素："+list.remove(new Person("lala",12)));
        System.out.println("判断该元素是否存在："+list.contains(new Person("kk",19)));
        System.out.println("当前数组长度为："+list.size());
    }
}
//当前数组长度为：5
//移除元素：true
//判断该元素是否存在：true
//当前数组长度为：4
```

#### 2.3LinkedList类---使用上与ArrayList一致

**从名称就可以看出该类底层是依靠链表(双向链表，默认尾插)实现。**

```java
import java.util.LinkedList;
import java.util.List;

class Person{
    private String name;
    private Integer age;

    public Person(String name, Integer age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
    @Override
    public boolean equals(Object obj) {
        if(obj==this){
            return true;
        }else if(obj==null||!(obj instanceof Person)){
            return false;
        }
        Person per=(Person) obj;
        return per.name.equals(this.name)&&per.age.equals(this.age);
    }
}
public class Test{
    public static void main(String[] args) {
        List<Person> list=new LinkedList<>();
        list.add(new Person("lala",12));
        list.add(new Person("kk",19));
        list.add(new Person("lala",11));
        list.add(new Person(null,9));
        list.add(new Person(null,null));
        System.out.println("当前数组长度为："+list.size());
        System.out.println("移除元素："+list.remove(new Person("lala",12)));
        System.out.println("判断该元素是否存在："+list.contains(new Person("kk",19)));
        System.out.println("当前数组长度为："+list.size());
    }
}
//当前数组长度为：5
//移除元素：true
//判断该元素是否存在：true
//当前数组长度为：4
```

通过对ArrayList、Vector、LinkedList类的简单使用，我们可以发现其在应用上无任何区别，使用remove()方法与contains()方法均要在自定义类中覆写equals()方法。

#### 那么就有这么一道经典面试题存在，请解释ArrayList、Vector、LinkedList三个类之间的区别？

我们可从ArrayList与Vector的区别以及Vector与LinkedList的区别两方面来谈：

**首先先来看ArrayList类与Vector类的区别：**

(1**)出现版本**：ArrayList:JDK1.2;Vector：JDK1.0,Vector类是老版本的动态数组实现类，出现在List、Collection接口之前。

(2)**初始化策略不同**：ArrayList:调用无参构造时并不会直接开辟空间，而是将数组长度置为空，在第一次添加元素时才初始化数组，并将其初始长度置为默认长度10；Vector类是调用无参构造直接开辟空间将数组长度置为10。

```java
//ArrayList
public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
public boolean add(E e) {
        ensureCapacityInternal(size + 1); //默认size=0
        elementData[size++] = e;
        return true;
    }
private void ensureCapacityInternal(int minCapacity) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {//当数组长度为空时才执行比较1与默认值大小取最大值
            minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);//取默认数组长度与1的最大值
        }
        ensureExplicitCapacity(minCapacity);//初始化时可能调用有参构造自行设定数组初始化长度或扩容后数组长度直接开辟对应长度即可
    }
private static final int DEFAULT_CAPACITY = 10;
//Vector
public Vector() {
        this(10);//构造方法重写
    }
public Vector(int initialCapacity) {
        this(initialCapacity, 0);
    }
public Vector(int initialCapacity, int capacityIncrement) {
        super();
        if (initialCapacity < 0)
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        this.elementData = new Object[initialCapacity];
        this.capacityIncrement = capacityIncrement;
    }
```

(3)**扩容机制不同**：ArrayList每次扩容为1.5倍；Vector每次扩容为2倍。

```java
//ArrayList
public boolean add(E e) {
        ensureCapacityInternal(size + 1);//(1)
        elementData[size++] = e;
        return true;
    }
private void ensureCapacityInternal(int minCapacity) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            minCapacity = Math.max(DEFAULT_CAPACITY, minCapacity);
        }

        ensureExplicitCapacity(minCapacity);(2)//非初始化数组长度
    }
private void ensureExplicitCapacity(int minCapacity) {
        modCount++;//修改数组次数

        // overflow-conscious code
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);(3)//扩容
    }
private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);//扩容为原来的1.5倍
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
//Vector
public synchronized boolean add(E e) {
        modCount++;
        ensureCapacityHelper(elementCount + 1);
        elementData[elementCount++] = e;
        return true;
    }
private void ensureCapacityHelper(int minCapacity) {
        // overflow-conscious code
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }
private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        //capacityIncrement默认小于0，相当于扩容为原来的2倍
        int newCapacity = oldCapacity + ((capacityIncrement > 0) ?
                                         capacityIncrement : oldCapacity);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
```

(4)**线程安全性**：ArrayList:采用异步处理，线程不安全，效率较高；Vector在方法上加锁，采用同步处理，线程安全，效率较低。

```java
//ArrayList:采用异步处理
public boolean add(E e) {
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        elementData[size++] = e;
        return true;
    }
//Vector：在方法上采用同步处理(同步方法)
public synchronized boolean add(E e) {
        modCount++;
        ensureCapacityHelper(elementCount + 1);
        elementData[elementCount++] = e;
        return true;
    }
```

(5)**遍历不同**：Vector类支持Enumeration(枚举输出)，而ArrayList不支持。

**ArrayList与Vector的共同点：底层均是依靠数组实现。**

#### ArrayList与LinkedList类的区别：

(1)底层实现：ArrayLIst底层依靠数组实现，LinkedList底层依靠双链表实现。

```java
//LinkedList
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

**(2)ArrayList时间复杂度为O(1)，LinkedList时间复杂度为O(n)。**

原因是访问数组元素可直接通过下标访问；但访问链表必须先进行遍历。

**ArrayList与LinkedList类的共同点：**

(1)均采用异步处理;

```java
//LinkedList
public boolean add(E e) {
        linkLast(e);//默认尾插
        return true;
    }
```

(2)均不支持枚举输出。

##### 总结：实现List接口的三大子类存储元素均是先进先出(FIFO),且使用remove()、contains()方法时必须要在自定义类中覆写equals()方法。

### 3.Set接口

**Set接口没有其扩展的方法，均是继承Collection接口中的方法。故Set接口中没有get()方法。**

Set集合不允许数据重复。Set接口有两个常用的子类：HashSet(无序存储)、TreeSet(有序存储)。

#### 3.1HashSet(无序存储)

HashSet简单实现自定义类数据存储：

```java

```

#### 3.2TreeSet(有序存储)

通过List接口的学习，我们知道ArrayList、Vector和LinkedList在使用上是一致的。那么以上HashSet的简单使用，直接替换为TreeSet类还可以正常运行吗？

```java
import java.util.HashSet;
import java.util.Set;
import java.util.TreeSet;

class Person{
    private String name;
    private Integer age;
    public Person(String name, Integer age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
public class Test{
    public static void main(String[] args) throws Exception {
        Set<Person> set=new TreeSet<>();
        set.add(new Person("小新",5));
        set.add(new Person("小葵",0));
        set.add(new Person(null,3));
        set.add(new Person("娜娜子",null));
        set.add(new Person(null,null));
        System.out.println(set);
    }
}
//Exception in thread "main" java.lang.ClassCastException: Person cannot be cast to java.lang.Comparable
	//at java.util.TreeMap.compare(TreeMap.java:1294)
	//at java.util.TreeMap.put(TreeMap.java:538)
	//at java.util.TreeSet.add(TreeSet.java:255)
	//at Test.main(Test.java:40)
//根据运行结果可知将HashSet直接替换为TreeSet时会报运行异常(类型转换异常)，Person cannot be cast to java.lang.Comparable表明Person类不支持排序；原因是TreeSet是有序存储，此代码并没有提供任何有关顺序的规定，故运行错误。
```

我们知道TreeSet为有序存储，HashSet为无序存储。那么这个序到底是指什么呢？

**Java中提供了两个接口Comparable(内部排序接口)、Compartor(外部排序接口)。自定义类直接实现Comparable接口或在自定义类的外部另外创建类实现Compartor接口后自定义类才可进行有序存储。我们将在自定义类的外部实现Compartor接口的类叫比较器。**

在了解TreeSet类之前先来认识一下这两个接口：

```java
//Comparable：该接口中只有compareTo一个方法，子类覆写该方法制定顺序规则(默认升序)
public interface Comparable<T> {
   public int compareTo(T o);
}
//Comparator:该接口中共有compare与equals两个抽象方法，还有一些普通方法。
//实现Comparator时只需覆写compare()方法制定顺序规则即可，因所有类均默认继承Object类，故equals()方法已在Object类中实现，不必再次覆写
public interface Comparator<T> {
    int compare(T o1, T o2);
    boolean equals(Object obj);
}
```

无论是comparable()方法还是compare()方法，均有一个int返回值，该返回值有如下三种情况：

```java
a.>0，当返回值>0时，表明前者大于后者
b.<0，当返回值<0时，表明前者小于后者
c.=0，两者相等
```

**3.2.1Comparable接口简单使用：内部比较，compareTo()方法只有一个参数是因为默认是自身跟其他相比**

```java
import java.util.Comparator;
import java.util.Set;
import java.util.TreeSet;

class Person implements Comparable<Person>{
    private String name;
    private Integer age;
    public Person(String name, Integer age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    @Override
    public int compareTo(Person o) {
        if(this.age>o.age){//当前对象大于比较对象时
            return 1;
        }else if (this.age<o.age){//当前对象小于比较对象时
            return -1;
        }else {
            return this.name.compareTo(o.name);//当age相等时返回比较name的大小
        }
    }
}
public class Test{
    public static void main(String[] args) throws Exception {
        Set<Person> set=new TreeSet<>();
        set.add(new Person("小新",5));
        set.add(new Person("小葵",0));
        set.add(new Person("小新",0));
        set.add(new Person("娜娜子",5));
        set.add(new Person("小新",5));
        System.out.println(set);
    }
}
//[Person{name='小新', age=0}, Person{name='小葵', age=0}, Person{name='娜娜子', age=5}, Person{name='小新', age=5}]
```

由以上代码输出结果我们可以看出，结果遵循compareTo()方法中定义的升序；但值得注意的是，当对象已经存在时，只会存储一次。

**3.2.2Comparator接口简单使用：外部比较，独立于自定义类之外，故compare()方法需要传入两个参数比较**

```java
import java.util.Comparator;
import java.util.Set;
import java.util.TreeSet;

class Person{
    private String name;
    private Integer age;
    public Person(String name, Integer age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
//升序
class AscCompare implements Comparator<Person>{
    @Override
    public int compare(Person o1, Person o2) {
        if(o1.getAge()>o2.getAge()){
            return 1;
        }else if(o1.getAge()<o2.getAge()){
            return -1;
        }else{
            return o1.getName().compareTo(o2.getName());
        }
    }
}
//降序
class DescCompare implements Comparator<Person>{
    @Override
    public int compare(Person o1, Person o2) {
        if(o2.getAge()>o1.getAge()){
            return 1;
        }else if(o2.getAge()<o1.getAge()){
            return -1;
        }else{
            return o2.getName().compareTo(o1.getName());
        }
    }
}
public class Test{
    public static void main(String[] args) throws Exception {
        Set<Person> set=new TreeSet<>(new DescCompare());//传入比较器对象
        set.add(new Person("小新",5));
        set.add(new Person("小葵",0));
        set.add(new Person("小新",0));
        set.add(new Person("娜娜子",5));
        set.add(new Person("小新",5));
        System.out.println(set);
    }
}
//升序
//[Person{name='小新', age=0}, Person{name='小葵', age=0}, Person{name='娜娜子', age=5}, Person{name='小新', age=5}]
//降序
//[Person{name='小新', age=5}, Person{name='娜娜子', age=5}, Person{name='小葵', age=0}, Person{name='小新', age=0}]
```

通过以上简单使用，我们可以发现

(1)Comparable接口是一个内部排序(自定义类直接实现此接口，在自定义类内部覆写compareTo()方法即可)，Comparator接口是外部排序(独立于自定义类外的一个类实现Comparator接口，该类被称为比较器)；

(2)通过TreeSet类实例化Set时，若调用TreeSet类/TreeMap类的无参构造，那么自定义类就一定要实现Comparable接口且覆写compareTo()方法；若调用TreeSet类/TreeMap类的有参构造，那么就一定要传入一个比较器的实例化对象；

(3)**TreeSet类必须依赖于compare()方法/compareTo()方法中的排序规则达到有序存储，故一般自定义类有多个属性时建议在比较方法中均要进行比较。**因若只有某一元素或部分元素进行比较时，只要参与比较的属性内容相同无论其他属性(不作为比较属性)内容是否相同均会认为该对象已存在，造成对象缺失的现象。

将上述代码的简单改动，对(3)的简单验证：

```java
class DescCompare implements Comparator<Person>{
    @Override
    public int compare(Person o1, Person o2) {
        return o2.getAge()-o1.getAge();
    }
}
//[Person{name='小新', age=5}, Person{name='小葵', age=0}]
```

我们根据输出结果可以看到，当比较方法中属性不足时，当比较属性相同时就认为该对象存在，造成数据缺失。

(4)**使用TreeSet类时自定义类属性值不能为null，否则会报NullPointerException(运行错误，空指针异常)。**其实严格意义上来讲是比较方法中作为比较属性的属性值不能为null，没有作为比较属性的属性可以为null，但又因比较方法中属性若不全则会出现对象缺失的清空，还是建议将所有属性均进行比较。

```java
//Person类与上文代码一致
class DescCompare implements Comparator<Person>{
    @Override
    public int compare(Person o1, Person o2) {
        if(o2.getAge()>o1.getAge()){
            return 1;
        }else if(o2.getAge()<o1.getAge()){
            return -1;
        }else{
            return o2.getName().compareTo(o1.getName());
        }
    }
}
public class Test{
    public static void main(String[] args) throws Exception {
        Set<Person> set=new TreeSet<>(new DescCompare());
        set.add(new Person("小新",5));
        set.add(new Person(null,null));//将属性值设为Null
        System.out.println(set);
    }
}
//Exception in thread "main" java.lang.NullPointerException
	//at DescCompare.compare(Test.java:53)
	//at DescCompare.compare(Test.java:49)
	//at java.util.TreeMap.put(TreeMap.java:552)
	//at java.util.TreeSet.add(TreeSet.java:255)
	//at Test.main(Test.java:66)


//当比较方法中只比较age属性时，name值为null可正常输出
class DescCompare implements Comparator<Person>{
    @Override
    public int compare(Person o1, Person o2) {
        return o2.getAge()-o1.getAge();
    }
}
public class Test{
    public static void main(String[] args) throws Exception {
        Set<Person> set=new TreeSet<>(new DescCompare());
        set.add(new Person("小新",5));
        set.add(new Person("娜娜子",10));
        set.add(new Person(null,6));
        System.out.println(set);
    }
}
//[Person{name='娜娜子', age=10}, Person{name='null', age=6}, Person{name='小新', age=5}]
```

#### Comparable接口VSComparator接口(重要面试)

在Java中，若想实现自定义类的比较，必须实现Comparable接口或Comparator接口。

**(1)Java.lang.Comparable接口(内部比较器)：**

a.若一个类实现了Comparable接口，就意味着该类支持排序。存放该类的Collection或数组，可以直接通过Collections.sort()或Arrays.sort()进行排序；

b.实现了Comparable接口的类可以直接存放在TreeSet或TreeMap中。

c.Comparable是排序接口，若一个类实现了Comparable接口，意味着该类支持排序，是一个内部比较器(自己和别人比)

**(2)java.util.Comparator接口(外部排序接口)**

a.若要控制某个自定义类的排序，而该类本身不支持排序(类本身没有实现Comparable接口)。我们可以建立一个该类的“比较器”来进行排序。比较器实现Comparator接口即可。

比较器：实现了Comparator接口的类作为比较器，通过该比较器来进行类的排序。

b.Comparator接口是比较器接口，类本身不支持排序，专门有若干个第三方的比较器(实现了Comparator接口的类)来进行类的排序，是一个外部比较器(策略模式)。

策略模式：实现了Comparator接口进行第三方排序。此方法更灵活，可以轻松改变策略进行第三方的排序算法。

#### HashSet VS TreeSet(重要面试)

(1)HashSet(无序存储)

a.底层使用哈希表+红黑树

b.允许存放null,无序存储

(2)TreeSet(有序存储)

a.底层使用红黑树

b.不允许出现空值，有序存储

c.自定义类要想保存到TreeSet中，要么实现Comparable接口，要么向TreeSet传入比较器(Comparator接口)

#### 3.3重复元素判断(HashSet)

Collection接口中存在remove()、contains()方法，即HashMap和TreeMap类中肯定也覆写了这两个方法。在List接口子类中使用这两个方法必须覆写equals()方法，那么在Set接口子类中要怎么使用呢？

通过上文对TreeSet的了解可以知道，**TreeSet/TreeMap依靠Comparable或Comparator接口来区分重复元素。**

可是HashSet与这两个接口并没有关系，所以HashSet判断重复元素的方式是依靠Object类中的两个方法：

```java
1.public boolean equals(Object obj) {}//对象内容比较
2.public native int hashCode();//hash码
```

**因为HashSet底层使用哈希表+红黑树实现，故去重需要两步：**

**a.调用hashCode()方法得到hash码确定该对象在哪个桶中存放；**

**b.再调用equals()方法判断该桶中是否有重复元素，若有，则忽略；若没有，则尾插入链表即可。**

![2](C:\Users\14665\source\JAVA学习\学习总结类集---List接口与Set接口\2.png)

TreeSet简单去重：

```java
import java.util.HashSet;
import java.util.Objects;
import java.util.Set;

class Person{
    private String name;
    private Integer age;
    public Person(String name, Integer age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person person = (Person) o;
        return Objects.equals(name, person.name) &&
                Objects.equals(age, person.age);
    }

    @Override
    public int hashCode() {

        return Objects.hash(name, age);
    }
}
public class Test{
    public static void main(String[] args) throws Exception {
        Set<Person> set=new HashSet<>();
        set.add(new Person("小新",5));
        set.add(new Person("娜娜子",10));
        System.out.println("当前长度："+set.size());
        System.out.println("判断是否存在该对象："+set.contains(new Person("小新",5)));
        System.out.println("移除对象："+set.remove(new Person("娜娜子",10)));
        System.out.println("当前长度："+set.size());
        System.out.println(set);
    }
}
//当前长度：2
//判断是否存在该对象：true
//移除对象：true
//当前长度：1
//[Person{name='小新', age=5}]
```

自定义类实现hashCode源码剖析：

```java
public int hashCode() {

        return Objects.hash(name, age);
    }
public static int hash(Object... values) {
        return Arrays.hashCode(values);
    }
public static int hashCode(Object a[]) {
        if (a == null)
            return 0;

        int result = 1;

        for (Object element : a)
            result = 31 * result + (element == null ? 0 : element.hashCode());//31为经验值

        return result;
    }
public native int hashCode();//本地方法
```

在进行去重时覆写了equals()方法，关于覆写此方法有以下几点原则：

```java
1.自反性。对于任何非空引用值x,x.equals(x)都返回true;
2.对称性。对于任何非空x,y,当且仅当x.equals(y)返回true,y.equals(x)也返回true;
3.传递性。对于任何非空的x,y,z，如果x.equals(y)返回true,y.equals(z)返回true,则一定有x.equals(z)返回true；
4.一致性。对于任何非空的x,y，若x与y中属性没有改变，则多次调用x.equals(y)始终返回true或false。
5.非空性。对于任何非空引用x，x.equals(null)一定返回false。
```

了解完了HashSet处理重复元素的方法，接下来我们通过以下几个经典面试题来做一总结：

#### 1）HashSet/HashMap是如何存放数据的？

首先调用hashCode()方法计算得到hash码确定该对象在哪个桶中存放；之后再调用equals()判断该桶中是否存在重复元素，若有则不再防止该元素，若没有则尾插入链表中。

#### 2）HashSet/HashMap为什么要同时覆写hashCode和equals?

原因是HashSet/HashMap底层依靠哈希表+红黑树实现，需要先调用hashCode()方法计算得到hash码确定该对象在哪个桶中存放；之后再调用equals()判断该桶中是否存在重复元素。

**若两个对象equals()方法返回true，它们的hashCode必然相等；但两个对象的hashCode相等，equals不一定相等。当且仅当equals与hashCode方法均返回true,才认为两个对象真正想等。**

#### 3）哈希表的存在意义？/为什么要分桶来存放数据？

**为了优化查找次数。因若只是利用数组存储的话，要判度数组中是否存在某元素的时间复杂度为O(n);分桶排序后查找是否存在某元素时可通过hash码快速确定桶的位置，之后遍历该桶中链表查找是否存在该元素(链表长度<8)，若链表长度>8时链表变为红黑树，遍历红黑树根据二分查找法，效率大大提高。**

![3](C:\Users\14665\source\JAVA学习\学习总结类集---List接口与Set接口\3.png)

### 4.集合输出

之前进行集合输出时都利用了toString()，或利用了List接口中的get()方法，这些都不是集合的标准输出。

从标准上来讲，集合输出一共有四种方式：**Iterator**、ListIterator、Enumeration、foreach。

#### 4.1迭代器：Iterator(重要)--只有Set和List接口支持，且只能从前到后输出(Collection接口提供)

迭代器：为了遍历集合而生。(迭代器模式)

**在JDK1.5之前，iterator()方法位于Collection接口中，通过此方法可以取得Iterator接口的实例化对象；在JDK1.5之后将此方法提升为Iterator接口中的方法。只要Collection接口有这个方法，那么List、Set也一定有此方法。**

```java
public interface Collection<E> extends Iterable<E> {}//Collection接口继承Iterator接口
```

我们先来学习一下最初设计Iterator接口中的几个重要方法：

```java
1.public boolean hasNext() {}//判断是否有下一个对象
2.public E next() {}//取得当前元素
3.public default void remove() {}//删除元素(此方法从JDK1.8开始变为default普通方法)
```

迭代器标准使用：

```java
public class Test{
    public static void main(String[] args) throws Exception {
        Set<String> set=new HashSet<>();
        set.add("小新");
        set.add("娜娜子");
        //1.获取Iterator对象
        Iterator<String> iterator=set.iterator();
        //2.输出
        while (iterator.hasNext()){//判断是否还有下一个元素
            //取得当前元素
            System.out.println(iterator.next());
        }
    }
}
//小新
//娜娜子
```

调用Collection集合子类的iterator方法取得内置的迭代器，使用以下输出格式：

```java
While(iterator.hasNext()){
     System.out.println(iterator.next());
}
```

那么使用用迭代器输出时要怎么删除集合中元素呢？

#### 删除元素：

```java
//若在迭代器输出中直接使用set.remove()会报错误
public class Test{
    public static void main(String[] args) throws Exception {
        Set<String> set=new HashSet<>();
        set.add("小新");
        set.add("娜娜子");
        //1.获取Iterator对象
        Iterator<String> iterator=set.iterator();
        //2.输出
        while (iterator.hasNext()){//判断是否还有下一个元素
            //直接使用remove()方法会抛出运行异常--并发修改异常
            set.remove("娜娜子");
            //取得当前元素
            System.out.println(iterator.next());
        }
    }
}
//Exception in thread "main" java.util.ConcurrentModificationException
	//at java.util.HashMap$HashIterator.nextNode(HashMap.java:1437)
	//at java.util.HashMap$KeyIterator.next(HashMap.java:1461)
	//at Test.main(Test.java:61)

//在迭代器输出中使用iterator.remove()则会删除成功
import java.util.ArrayList;
import java.util.Collections;
import java.util.Iterator;
import java.util.List;

public class Test{
    public static void main(String[] args){
        List<String> list=new ArrayList<>();
        Collections.addAll(list,"小新","娜娜子","小葵","小新");
        Iterator<String> iterator=list.iterator();
        while (iterator.hasNext()){
            String str=iterator.next();
            if (str.equals("小新")){
                iterator.remove();
                continue;
            }
            System.out.println(str);
        }
    }
}
//娜娜子
//小葵
```

以上代码输出原因是因为Java存在**fail-fast机制(快速失败机制)**。

#### fail-fast机制：ConcurrentModificationException(并发修改异常)发生在Collection集合使用迭代器遍历时，使用集合类提供的修改集合内容的方法报错。而使用迭代器的remove()方法不会报错。

那么为什么使用集合类的remove方法会报错，而使用迭代器的remove方法不会报错呢？接下来我们看源码：

```java
1.出现了异常之后我们要明确出现异常的位置，最上面报错位置就是该异常的源发地(ArrayList.java:901)
Exception in thread "main" java.util.ConcurrentModificationException
	at java.util.ArrayList$Itr.checkForComodification(ArrayList.java:901)
	at java.util.ArrayList$Itr.next(ArrayList.java:851)
	at Test.main(Test.java:13)
2.点击进入ArrayList.java中
final void checkForComodification() {
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
        }
3.从第二步可以看出判断条件若不满足，则会抛出并发修改异常，那么判断条件是什么意思呢？
protected transient int modCount = 0;//modCount该变量表示的是该集合被修改的次数
//add添加元素源码
private void ensureExplicitCapacity(int minCapacity) {
        modCount++;//添加一次modCount增加1，添加元素也算修改集合的一种方式

        // overflow-conscious code
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }
//集合类remove方法
public E remove(int index) {
        rangeCheck(index);

        modCount++;//移除一次modCount增加1，移除元素也算修改集合的一种方式
        E oldValue = elementData(index);

        int numMoved = size - index - 1;
        if (numMoved > 0)
            System.arraycopy(elementData, index+1, elementData, index,
                             numMoved);
        elementData[--size] = null; // clear to let GC do its work

        return oldValue;
    }
    //expectedModCount是迭代器中记录集合的修改次数，为ArrayList内置迭代器中的一个普通属性(为modCount的副本)，该属性初始化在调用构造方法时(即new Itr时)
private class Itr implements Iterator<E> {
        int expectedModCount = modCount;
        }
 
//每次取得当前元素时均先会判断modCount 与 expectedModCount是否相等，若不相等直接抛出异常
public E next() {
            checkForComodification();
            int i = cursor;
            if (i >= size)
                throw new NoSuchElementException();
            Object[] elementData = ArrayList.this.elementData;
            if (i >= elementData.length)
                throw new ConcurrentModificationException();
            cursor = i + 1;
            return (E) elementData[lastRet = i];
        }
```

通过源码的解读我们就可以知道modCount每修改一次集合均会+1，而expectedModCount是在list.iterator()集合获得迭代器对象时expectedModCount = modCount。所以在输出过程中直接使用集合类的remove()方法会导致modCount+1，因此modCount != expectedModCount抛出中断异常。

那么使用迭代器中的remove方法为什么又会成功呢？

```java
public void remove() {
            if (lastRet < 0)
                throw new IllegalStateException();
            checkForComodification();

            try {
                ArrayList.this.remove(lastRet);
                cursor = lastRet;
                lastRet = -1;
                expectedModCount = modCount;//迭代器的remove方法中修改集合后会将最新的modCount赋值给expectedModCount,故修改成功
            } catch (IndexOutOfBoundsException ex) {
                throw new ConcurrentModificationException();
            }
        }
```

总结：

**1.fail-fast机制中的modCount表示当前集合修改次数(添加元素、删除元素也是修改集合的方式)，每修改一次一次就+1；expectedModCount是迭代器中记录当前集合的修改次数，当取得集合迭代器时(即调用list.iterator())，expectedModCount = modCount，换言之，迭代器就是当前集合的一个副本。在集合输出时直接调用集合类的remove方法时expectedModCount不会更新故抛出并发修改异常，使用迭代器的remove方法时expectedModCount更新，故可删除成功。**

2.ConcurrentModificationException异常是在迭代器对象调用next()方法取得当前对象时抛出；

3.设计fail-fast主要是出于安全性考虑，因若在输出过程中有人通过add/remove修改集合，则可直接抛出异常，表明用户此时看到的数据可能不是最新的。

**4.快速失败策略保证了所有用户在进行迭代器遍历时拿到的数据是最新的，避免“脏读”产生。**

5.对应fail-fast机制还存在一个fail-safe机制。fail-safe机制是指不产生ConcurrentModificationException异常(如juc包下的所有线程安全集合(CopyOnWriteArrayList))。

6.以后在迭代器遍历时，不要修改集合内容。若要修改请在迭代器遍历前或之后进行，保证每次迭代输出时数据是最新的。

### 4.2双向迭代接口：ListIterator(List接口提供，Set不支持)

我们上文学到的Iterator迭代器输出有一个特点：只能从前向后遍历输出。若想进行双向迭代，必须依靠于Iterator接口的子接口ListIterator实现。

先来学习一下ListIterator接口的重要方法：

```java
1.public boolean hasNext();//判断是否还有下一个元素
2.public E next();//取得当前元素
3.public boolean hasPrevious();//判断是否还有上一个元素
4.public E previous();//取得上一个元素
```

注意：Iterator接口对象是由Collection接口支持的，但是ListIterator是由List接口支持的，List接口提供如下方法：

```java
public ListIterator listIterator();//取得ListIterator接口对象
```

ListIterator接口的简单使用:

```java
import java.util.*;

public class Test{
    public static void main(String[] args){
        List<String> list=new ArrayList<>();
        Collections.addAll(list,"小新","娜娜子","小葵","小新");
        ListIterator<String> listIterator=list.listIterator();
        System.out.println("从前向后遍历输出：");
        while (listIterator.hasNext()){
            System.out.print(listIterator.next());
        }
        System.out.println();
        System.out.println("从后向前遍历输出:");
        while (listIterator.hasPrevious()){
            System.out.print(listIterator.previous());
        }
    }
}
//从前向后遍历输出：
//小新娜娜子小葵小新
//从后向前遍历输出:
//小新小葵娜娜子小新
```

#### 但是使用双向迭代接口最要注意的一点是要想实现从后向前的输出，首先必须至少要从前向后遍历一次才可使用。

```java
//先使用从后向前遍历
import java.util.*;

public class Test{
    public static void main(String[] args){
        List<String> list=new ArrayList<>();
        Collections.addAll(list,"小新","娜娜子","小葵","小新","野原");
        ListIterator<String> listIterator=list.listIterator();
        System.out.println("从后向前遍历输出:");
        while (listIterator.hasPrevious()){
            System.out.print(listIterator.previous());
        }
        System.out.println();
        System.out.println("从前向后遍历输出：");
        while (listIterator.hasNext()){
            System.out.print(listIterator.next());
        }
    }
}
//从后向前遍历输出:
//
//从前向后遍历输出：
//小新娜娜子小葵小新野原
```

### 4.3Enumeration枚举输出(JDK1.0)只有Vector类支持

**Enumeration接口在JDK1.0时就有，要想取得这个接口的实例化对象，只能依靠Vector子类，因Enumeration在最早设计时就是为Vector服务的，**在Vector类中提供一个取得Enumeration接口对象的方法：

```java
public Enumeration elements();//取得Enumeration对象(JDK1.5)
```

JDK1.5对Enumeration做了更改，主要是追加了泛型的应用。接下来学习一下Enumeration接口中的方法：

```java
public interface Enumeration<E> {
     boolean hasMoreElements();//判断是否有下一个元素
     E nextElement();//取得当前元素
}
```

Enumeration接口的简单使用：

```java
import java.util.*;

public class Test{
    public static void main(String[] args){
        Vector<String> vector=new Vector<>();
        Collections.addAll(vector,"拉拉","kk","oo");
        Enumeration<String> enumeration=vector.elements();
        while (enumeration.hasMoreElements()){
            System.out.println(enumeration.nextElement());
        }
    }
}
//拉拉
//kk
//oo
```

Enumeration的应用场景：使用第三方的库，版本较老，不支持Iterator。

### 4.4foreach输出--所有子类均满足

从JDK1.5开始foreach可以输出数组，也可输出集合。

##### 能使用foreach输出的本质在于各个集合类都内置了迭代器。

foreach输出集合的简单使用：

```java
import java.util.*;

public class Test{
    public static void main(String[] args){
        List<Integer> list=new ArrayList<>();
        Collections.addAll(list,1,3,4,5,9,6,7);
        for (Integer i:list ) {
            System.out.print(i+" ");
        }
    }
}
//1 3 4 5 9 6 7 
```

