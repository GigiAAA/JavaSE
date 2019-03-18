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

