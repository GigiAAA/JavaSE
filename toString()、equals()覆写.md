```java
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

equals()方法覆写

