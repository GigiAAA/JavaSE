### 泛型实现数组动态增删查改

```java
//Sequence.java
//线性表规范
public interface Sequence<T,E> {//泛型接口
    /**
     * 向线性表中添加元素
     * @param data 要存储的元素
     */
    void add(E data);

    /**
     * 线性表中删除元素
     * @param index 要删除的元素下标
     * @return 是否删除成功
     */
    boolean remove(int index);

    /**
     * 在线性表中查找指定索引的元素
     * @param index 要查找的索引
     * @return
     */
    E get(int index);

    /**
     * 判断线性表中是否有指定元素
     * @param data 要查找的元素内容
     * @return
     */
    boolean contains(E data);

    /**
     * 修改线性表中指定索引的内容
     * @param index 要修改的元素下标
     * @param newData 修改后的内容
     * @return
     */
    E set(int index,E newData);

    /**
     * 返回当前线性表元素个数
     * @return
     */
    int size();

    /**
     * 直接清空线性表内容
     */
    void clear();

    /**
     * 将线性表转为数组
     * @return
     */
    E[] toArray();
}

//SequenceImpl.java

import java.util.Arrays;

public class SequenceImpl<T,E> implements Sequence<T,E> {//泛型类实现泛型接口时保留泛型
    private E[] array;//定义泛型数组
    public static final int MAX_CARACITY=Integer.MAX_VALUE-8;//整型最大值
    private Integer size=0;

    public E[] getArray() {
        return array;
    }

    public void setArray(E[] array) {
        this.array = array;
    }

    @Override
    public void add(E data) {
        ensureArrayNumber(size+1);
        array[size++]=data;
    }

    @Override
    public boolean remove(int index) {
        rangeCheck(index);
        int moveSteps=size-index-1;
        if(moveSteps>0){
            System.arraycopy(array,index,array,index+1,moveSteps);
        }
        array[--size]=null;
        return true;
    }

    @Override
    public E get(int index) {
        rangeCheck(index);
        return array[index];
    }

    @Override
    public boolean contains(E data) {
        for(int i=0;i<size;i++){
            if(array[i]==data){
                return true;
            }
        }
        return false;
    }

    @Override
    public E set(int index, E newData) {
        rangeCheck(index);
        E oldData=array[index];
        array[index]=newData;
        return oldData;
    }

    @Override
    public int size() {
        return this.size;
    }

    @Override
    public void clear() {
        for(int i=0;i<size;i++){
            array[i]=null;
        }
        this.size=0;
    }

    @Override
    public E[] toArray() {
        return array;
    }
    private void ensureArrayNumber(int cap){
        if(cap-array.length>0){
            grow(cap);
        }
    }
    private void grow(int cap){
        int oldData=array.length;
        int newData=oldData<<1;
        if(cap-newData>0){
            newData=cap;
        }
        if(newData-MAX_CARACITY>0){
            throw new IndexOutOfBoundsException("数组越界异常！");
        }
        array=Arrays.copyOf(array,newData);
    }
    private void rangeCheck(int index){
        if(index<0||index>=size){
            throw new IndexOutOfBoundsException("索引异常！");
        }
    }
    public void display(){
        for(Object i:array){
            System.out.print(i+" ");
        }
    }
}

//Study.java
public class Study {
    public static void main(String[] args){
        Sequence<Integer,String>sequence=new SequenceImpl<>();//实例化对象时确定类型
        String[] array={null,null,null};//初始化数组
        ((SequenceImpl<Integer, String>) sequence).setArray(array);//通过setter方法初始化数组
        sequence.add("蛋哥1");
        sequence.add("蛋哥2");
        sequence.add("蛋哥3");
        ((SequenceImpl<Integer, String>) sequence).display();
        System.out.println(" ");
        System.out.println(sequence.contains("蛋哥1"));
        System.out.println(sequence.contains("8"));
        System.out.println(sequence.get(0));
        System.out.println(sequence.set(1,"DDD"));
        System.out.println(sequence.get(1));
        sequence.remove(2);
        ((SequenceImpl<Integer, String>) sequence).display();
        sequence.clear();
        System.out.println(sequence.size());
    }
}

//蛋哥1 蛋哥2 蛋哥3  
//true
//false
//蛋哥1
//蛋哥2
//DDD
//蛋哥1 DDD null 0
```

### 泛型实现单链表动态增删查改

```java
//Sequence.java
//线性表规范
public interface Sequence<T> {
    /**
     * 向线性表中添加元素
     * @param data 要存储的元素
     */
    void add(T data);

    /**
     * 线性表中删除元素
     * @param index 要删除的元素下标
     * @return 是否删除成功
     */
    boolean remove(int index);

    /**
     * 在线性表中查找指定索引的元素
     * @param index 要查找的索引
     * @return
     */
    T get(int index);

    /**
     * 判断线性表中是否有指定元素
     * @param data 要查找的元素内容
     * @return
     */
    boolean contains(T data);

    /**
     * 修改线性表中指定索引的内容
     * @param index 要修改的元素下标
     * @param newData 修改后的内容
     * @return
     */
    T set(int index,T newData);

    /**
     * 返回当前线性表元素个数
     * @return
     */
    int size();

    /**
     * 直接清空线性表内容
     */
    void clear();

    /**
     * 将线性表转为数组
     * @return
     */
    T[] toArray();
}


//LinkedListImpl.java
public class LinkedListImpl<T> implements Sequence<T> {
    private Integer size=0;
    private ListNode head;
    T[] data;

    public T[] getData() {
        return data;
    }

    public void setData(T[] data) {
        this.data = data;
    }

    class ListNode{
        ListNode next;
        T data;
        public ListNode(ListNode next, T data) {
            this.next = next;
            this.data = data;
        }

        public ListNode() {
        }
    }
    @Override
    public void add(T data) {
        ListNode newNode=new ListNode(head,data);
        head=newNode;
        size++;
    }

    @Override
    public boolean remove(int index) {
        if(head==null){
            return false;
        }
        rangeCheck(index);
        ListNode cul=node(index);
        ListNode dummyHead=new ListNode();
        dummyHead.next=head;
        ListNode prev=dummyHead;
        for(int i=0;i<index;i++){
            prev=prev.next;
        }
        prev.next=cul.next;
        cul.data=null;
        size--;
        return true;
    }

    @Override
    public T get(int index) {
        rangeCheck(index);
        T listData=node(index).data;
        return listData;
    }

    @Override
    public boolean contains(T data) {
        for(int i=0;i<size;i++){
            if(data.equals(node(i).data)){
                return true;
            }
        }
        return false;
    }

    @Override
    public T set(int index, T newData) {
        rangeCheck(index);
        T oldData=node(index).data;
        node(index).data=newData;
        return oldData;
    }

    @Override
    public int size() {
        return this.size;
    }

    @Override
    public void clear() {
        ListNode prev=head;
        for(ListNode temp=prev;temp!=null;temp=temp.next){
            if(head.next!=null){
                head=head.next;
                temp.data=null;
            }else {
                head.data=null;
            }
            size--;
        }
    }

    @Override
    public T[] toArray() {
        int i=0;
        for(ListNode temp=head;temp!=null;temp=temp.next){
            data[i++]=temp.data;
        }
        return data;
    }
    private ListNode node(int index){
        ListNode temp=head;
        for(int i=0;i<index;i++){
            temp=temp.next;
        }
        return temp;
    }
    public void display(){
        for(ListNode temp=head;temp!=null;temp=temp.next){
            System.out.print(temp.data+" ");
        }
    }
    private void rangeCheck(int index){
        if(index<0&&index>=size){
            throw new IndexOutOfBoundsException("越界异常");
        }
    }
}


//Study.java

public class Study {
    public static void main(String[] args){
        Sequence<Integer> sequence=new LinkedListImpl<>();
        int size=sequence.size();
        Integer[]data=new Integer[size];
        ((LinkedListImpl<Integer>) sequence).setData(data);
        sequence.add(1);
        sequence.add(2);
        sequence.add(3);
        ((LinkedListImpl) sequence).display();
        System.out.println(" ");
        System.out.println(sequence.get(1));
        sequence.set(0,99);
        System.out.println(sequence.get(0));
        System.out.println(sequence.contains(99));
        System.out.println(sequence.contains(9));
        sequence.remove(1);
        ((LinkedListImpl) sequence).display();
        System.out.println(" ");
        System.out.println(sequence.size());
        sequence.clear();
        System.out.println(sequence.size());
    }
}

//3 2 1  
//2
//99
//true
//false
//99 1  
//2
//0
```

### 泛型实现双链表动态增删查改

````java
//Sequence.java
//线性表规范
public interface Sequence<T> {
    /**
     * 向线性表中添加元素
     * @param data 要存储的元素
     */
    void add(T data);

    /**
     * 线性表中删除元素
     * @param index 要删除的元素下标
     * @return 是否删除成功
     */
    boolean remove(int index);

    /**
     * 在线性表中查找指定索引的元素
     * @param index 要查找的索引
     * @return
     */
    T get(int index);

    /**
     * 判断线性表中是否有指定元素
     * @param data 要查找的元素内容
     * @return
     */
    boolean contains(T data);

    /**
     * 修改线性表中指定索引的内容
     * @param index 要修改的元素下标
     * @param newData 修改后的内容
     * @return
     */
    T set(int index,T newData);

    /**
     * 返回当前线性表元素个数
     * @return
     */
    int size();

    /**
     * 直接清空线性表内容
     */
    void clear();

    /**
     * 将线性表转为数组
     * @return
     */
    T[] toArray();
}

//LinkedListImpl.java
public class LinkedListImpl<T> implements Sequence<T> {
    private ListNode head;
    private ListNode tail;
    private Integer size=0;
    private T[] array;

    public T[] getArray() {
        return array;
    }

    public void setArray(T[] array) {
        this.array = array;
    }

    class ListNode{
        T data;
        ListNode next;
        ListNode prev;

        public ListNode(T data, ListNode next, ListNode prev) {
            this.data = data;
            this.next = next;
            this.prev = prev;
        }

        public ListNode() {
        }
    }
    @Override
    public void add(T data) {
        ListNode newNode=new ListNode(data,null,tail);
        if(head==null){
            head=newNode;
        }else{
            tail.next=newNode;
        }
        tail=newNode;
        size++;
    }

    @Override
    public boolean remove(int index) {
        if(head==null){
            return false;
        }
        rangeCheck(index);
        ListNode cul=node(index);
        ListNode next=cul.next;
        ListNode prev=cul.prev;
        ListNode dummyHead=new ListNode();
        dummyHead.next=head;
        ListNode node=dummyHead;
        for(ListNode temp=head;temp!=cul;temp=temp.next){
            node=node.next;
        }
        if(prev==null){
            head=next;
            next=null;
        }else {
            node.next=next;
            prev=null;
        }
        if(next!=null){
            next.prev=prev;
            next=null;
        }
        cul.data=null;
        size--;
        return true;
    }

    @Override
    public T get(int index) {
        rangeCheck(index);
        return node(index).data;
    }

    @Override
    public boolean contains(T data) {
        for(int i=0;i<size;i++){
            if(data.equals(node(i).data)){
                return true;
            }
        }
        return false;
    }

    @Override
    public T set(int index, T newData) {
        rangeCheck(index);
        T oldData=node(index).data;
        node(index).data=newData;
        return oldData;
    }

    @Override
    public int size() {
        return this.size;
    }

    @Override
    public void clear() {
        ListNode dummyHead=new ListNode();
        dummyHead.next=head;
        if(head.next==null){
            head.data=null;
        }else{
            for(ListNode temp=head;temp!=null;temp=temp.next){
                if(head.next!=null){
                    head=head.next;
                    temp.prev=null;
                }
                temp.data=null;
                size--;
            }
        }
    }

    @Override
    public T[] toArray() {
        int i=0;
        for(ListNode temp=head;temp!=null;temp=temp.next){
            array[i++]=temp.data;
        }
        return array;
    }
    private void rangeCheck(int index){
        if(index<0||index>=size){
            throw new IndexOutOfBoundsException("越界异常");
        }
    }
    private ListNode node(int index){
        ListNode cul=head;
        ListNode cull=head;
        if(cul!=null&&cull!=null&&cul.next!=null){
            cul=cul.next.next;
            cull=cull.next;
        }
        int k=0;
        for(ListNode temp=head;temp!=cull;temp=temp.next){
            k++;
        }
        ListNode p=head;
        ListNode q=tail;
        if(index>k){
            for(int i=size-1;i>index;i--){
                q=q.prev;
            }
            return q;
        }else{
            for(int i=0;i<index;i++){
                p=p.next;
            }
            return p;
        }
    }
    public void display(){
        for(ListNode temp=head;temp!=null;temp=temp.next){
            System.out.print(temp.data+" ");
        }
    }
}


//Study.java

public class Study {
    public static void main(String[] args){
        Sequence<String> sequence=new LinkedListImpl<>();
        int size=sequence.size();
        String[] array=new String[size];
        ((LinkedListImpl<String>) sequence).setArray(array);
        sequence.add("罗志祥");
        sequence.add("小猪");
        sequence.add("xixi");
        ((LinkedListImpl) sequence).display();
        System.out.println();
        System.out.println(sequence.get(0));
        sequence.set(2,"红雷");
        System.out.println(sequence.get(2));
        sequence.remove(1);
        ((LinkedListImpl) sequence).display();
        System.out.println();
        System.out.println(sequence.contains("luozhixiang"));
        System.out.println(sequence.contains("罗志祥"));
        sequence.clear();
        System.out.println(sequence.size());
    }
}

//罗志祥 小猪 xixi 
//罗志祥
//红雷
//罗志祥 红雷 
//false
//true
//0
````

