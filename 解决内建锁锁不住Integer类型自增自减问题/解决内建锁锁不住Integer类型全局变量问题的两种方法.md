### 解决内建锁锁不住Integer类型全局变量自增自减问题的两种方法

了解这种问题的解决方法之前，我们先来看一下产生此种问题的原因

```java
class MyThread implements Runnable{
    static Integer num=0;

    public static Integer getNum() {
        return num;
    }

    @Override
    public void run() {
        for(int i=0;i<10000;i++){
            synchronized (num){
                num++;
            }
        }
    }
}
public class ThreadTest {
    public static void main(String[] args) throws InterruptedException {
        MyThread myThread=new MyThread();
        Thread threadA=new Thread(myThread,"线程A");
        Thread threadB=new Thread(myThread,"线程B");
        threadA.start();
        threadB.start();
        threadA.join();
        threadB.join();
        System.out.println("num最终结果为："+MyThread.getNum());
    }
}
//num最终结果为：15692
//num最终结果为：16178
//...每次输出结果均不同，但并不与预期结果20000相同
```

如上代码中num为静态变量，多个线程对同名变量进行修改操作时虽然使用内建锁对其进行上锁，但通过运行结果我们可以看出上锁失败依旧存在多并发问题。那么出现该问题的原因是什么呢？我们通过学习JMM内存模型可以知道，内存分为主内存与工作内存，工作内存中为主内存变量的副本，主内存与工作内存之间的关系为若某一线程要对变量进行修改操作，那么一定是先将该变量内容拷贝至自身的工作内存中，修改完毕后再将修改完毕的最新值拷贝至主内存中。那么此种执行模式在多线程情况下就势必存在数值的延迟问题，故导致两个线程对同名变量修改后与预期结果不同。

全局变量自增自减(num++,num--)实际上这是存在两步运算的，所以就极有可能线程A在进行完加法后，线程B由于系统调度的原因执行完毕将最新数值拷贝回主内存，之后再次执行线程A，此时线程A不会从主线程中拷贝最新数值而是接着执行赋值运算，执行完毕后再将数据拷贝回主内存，此时变量内容就出现了偏差。

![1](C:\Users\14665\source\JAVA学习\解决内建锁锁不住Integer类型自增自减问题\1.png)

关于以上问题还存在一个现象不得不注意，当我们将变量自增自减的范围修改一下时，结果又会正确。

```java
class MyThread implements Runnable{
    static Integer num=0;

    public static Integer getNum() {
        return num;
    }

    @Override
    public void run() {
        synchronized (num){
            for(int i=0;i<127;i++){
                num++;
            }
        }
    }
}
public class ThreadTest {
    public static void main(String[] args) throws InterruptedException {
        MyThread myThread=new MyThread();
        Thread threadA=new Thread(myThread,"线程A");
        Thread threadB=new Thread(myThread,"线程B");
        threadA.start();
        threadB.start();
        threadA.join();
        threadB.join();
        System.out.println("num最终结果为："+MyThread.getNum());
    }
}
//num最终结果为：254
```

看到这个范围127有没有很熟悉呢？Integer类为包装类，在[-128,127]区间数据存储于常量池中，也就是说当基本类型在此范围时，数值进行运算时进行自动装箱封装至同一对象中，用内建锁锁变量就相当于锁了同一对象，故任意时刻都只有一个线程能获取锁，运算结果正确。超出此范围是自动装箱过程实际上是在不断地创建新对象，也就是说此时锁变量锁的一定不是同一对象，故运算结果出现偏差。



那么解决上述问题的方法是哪些呢？怎样才能使出现此种现状时线程安全。

**第一种方法：锁类.class对象，此种方式一定能锁住，原因是任何一个对象在JVM中有且只有一个class对象。**

```java
class MyThread implements Runnable{
    static Integer num=0;

    public static Integer getNum() {
        return num;
    }

    @Override
    public void run() {
        synchronized (MyThread.class){//内建锁锁MyThread类的class对象
            for(int i=0;i<20000;i++){
                num++;
            }
        }
    }
}
public class ThreadTest {
    public static void main(String[] args) throws InterruptedException {
        MyThread myThread=new MyThread();
        Thread threadA=new Thread(myThread,"线程A");
        Thread threadB=new Thread(myThread,"线程B");
        threadA.start();
        threadB.start();
        threadA.join();
        threadB.join();
        System.out.println("num最终结果为："+MyThread.getNum());
    }
}
//num最终结果为：40000
```

**第二种方法：创建任意一个对象，内建锁锁此对象即可(因只有一个该对象)。**

```java
class MyThread implements Runnable{
    static Integer num=0;
    private final Object numLock=new Object();//一般创建此种对象时使用final关键字，因不可修改

    public static Integer getNum() {
        return num;
    }

    @Override
    public void run() {
        synchronized (numLock){
            for(int i=0;i<20000;i++){
                num++;
            }
        }
    }
}
public class ThreadTest {
    public static void main(String[] args) throws InterruptedException {
        MyThread myThread=new MyThread();
        Thread threadA=new Thread(myThread,"线程A");
        Thread threadB=new Thread(myThread,"线程B");
        threadA.start();
        threadB.start();
        threadA.join();
        threadB.join();
        System.out.println("num最终结果为："+MyThread.getNum());
    }
}
//num最终结果为：40000
```

但切记，此种多线程并发情况下变量加volatile关键字修饰是没有用的，虽说保证了可见性，但锁没有锁住，结果还是有误差。

