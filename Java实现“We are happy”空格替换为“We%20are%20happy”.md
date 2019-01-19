### Java实现“We are happy”空格替换为"We%20are%20happy"

#### 问题描述：

请实现一个程序，把字符串中的每个空格替换成“%20”。例如，输入“We are happy”,则输出"We%20are%20happy"。

#### 问题思路：

**注意：此题替换前后字符串长度不同，替换后的字符串长度大于原字符串长度。**

此题**我们可以创建新字符串，**在新字符串上替换，此时我们可自定义新建字符串长度。

**解题方法考虑思路：**

1.构造类，将原字符串转换为字符数组；

2.新建字符数组（合理自定义长度），非空格处正常输出，空格处替换为‘%’,空格下一个字节替换为‘2’，再下一个字节替换为‘0’。

3.新建字符数组转换为字符串输出。

**解题方法源代码如下：**

```java
class EmptyInstand{
    public void charEmptyInstand(String str){
        char[] dataA=str.toCharArray();//原字符串转换为字符数组
        char[] dataB=new char[20];//新建字符数组
        int j=0;
        for(int i=0;i<dataA.length;i++){
            char c=dataA[i];
            if(c==' '){
                dataB[j]='%';
                dataB[j+=1]='2';//此处必须用j+=1表示下一个字节，若直接使用j+1没有赋值过程，j值不改变，故不能表示下一个字节
                dataB[j+=2]='0';
            }else{
                dataB[j]=c;
            }
            j++;
        }
        System.out.println(new String(dataB));//新建字符数组转换为字符串输出
    }
}
public class Test{
    public static void main(String[] args) {
        EmptyInstand emptyInstand=new EmptyInstand();
        emptyInstand.charEmptyInstand("We are happy");
    }
}

//输出结果为We%20are%20happy
```

