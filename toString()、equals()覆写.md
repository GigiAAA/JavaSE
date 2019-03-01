```java
//equals()方法覆写
class Person{
    private String name;
    private Integer age;
    public Person(String name,Integer age){
        this.name=name;
        this.age=age;
    }
    public boolean equals(Object obj){
        if(obj==null){
            return false;
        }
        if(this==obj){
            return true;
        }
        if(!(obj instanceof Person)){
            return false;
        }
        Person person=(Person)obj;
        return this.name.equals(person.name)&&this.age.equals(person.age);
    }
}
    public class Test{
        public static void main(String[] args) {
           Person str1=new Person("小黑",89);
           Person str2=new Person("小黑",89);
           System.out.println(str1.equals(str2));
        }
    }
```

```java
//toString()方法覆写
    package JavaIDEAStudy;

class Person{
    private String name;
    private int age;
    public Person(String name,int age){
        this.name=name;
        this.age=age;
    }
    public String toString(){
        return "姓名为："+this.name+" 年龄为："+this.age;
    }
}
    public class Test{
        public static void main(String[] args) {
            Person person=new Person("小黑",89);
            System.out.println(person.toString());
        }
    }

```

instanceof关键字：

instanceof关键字为JAVA保留关键字。

作用为：判断其左边对象是否为其右边类的实例，返回boolean类型数据。可判断继承中的子类的实例是否为父类的实现。相当于C#中的is操作符。java中的instanceof是通过返回一个布尔值来指出这个对象是否是这个特定类或者是它子类的一个实例。