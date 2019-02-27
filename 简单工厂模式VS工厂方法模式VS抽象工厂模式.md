### 简单工厂模式

### 特点：

1.一个产品抽象接口（类）
2.若干个具体产品类
3.一个工厂（生产所有商品）
优点：简单易于实现，把类的实例化交给工厂，易于解耦。
缺点：违反了OCP原则。

```java
import java.util.Scanner;

interface IComputeFastory{//抽象产品接口
	void customWantsCompute();
}
//具体产品类
class MacImpl implements IComputeFastory{
    @Override
    public void customWantsCompute() {
        System.out.println("生产一台mac电脑");
    }
}
class ThinkPadImpl implements IComputeFastory{
    @Override
    public void customWantsCompute() {
        System.out.println("生产一台联想电脑");
    }
}
class ComputeFastory{//工厂类
        public static IComputeFastory getComputeInfo(String type){
            IComputeFastory iComputeFastory=null;
            if(type.equalsIgnoreCase("mac")){
                iComputeFastory=new MacImpl();
            }else if(type.equalsIgnoreCase("thinkPad")){
                iComputeFastory=new ThinkPadImpl();
            }
            return iComputeFastory;
        }
}
    public class Test{//客户端
        public void buyCompute(IComputeFastory iComputeFastory){
            iComputeFastory.customWantsCompute();
        }
        public static void main(String[] args) {
            Test test=new Test();
            Scanner scanner=new Scanner(System.in);
            System.out.println("请输入你想购买电脑的品牌名称：");
            String compute=scanner.nextLine();
            IComputeFastory iComputeFastory=ComputeFastory.getComputeInfo(compute);
            test.buyCompute(iComputeFastory);
        }
    }
```

### 工厂方法模式

特点：针对每个产品提供一个类，在客户端中判断使用哪个工厂类去创建对象。
组成：

1.一个抽象产品类
2.多个具体产品类
3.一个抽象工厂
4.多个具体工厂，每个具体工厂对应一个具体工厂
优点：降低了代码耦合度，对象的生成交与子类完成；实现了OCP原则，每次添加子产品不需要修改源代码。
缺点：代码量增大，每个具体产品需要一个具体工厂；当增加抽象产品，也就是添加一个其他产品类时，需要修改工厂，违背OCP原则

  ```java
interface IPrintCompute{
	void customWantsCompute();
}
class MacImpl implements IPrintCompute{
    @Override
    public void customWantsCompute() {
        System.out.println("生产一台mac电脑");
    }
}
class ThinkPadImpl implements IPrintCompute{
    @Override
    public void customWantsCompute() {
        System.out.println("生产一台联想电脑");
    }
}
interface IComputeFastory{
        IPrintCompute createCompute();
}
class MacFastoryImpl implements IComputeFastory{
    @Override
    public IPrintCompute createCompute() {
        return new MacImpl();
    }
}
class ThinkPadFastoryImpl implements IComputeFastory{
    @Override
    public IPrintCompute createCompute() {
        return new ThinkPadImpl();
    }
}
    public class Test{
        public void buyCompute(IPrintCompute iPrintCompute){
            iPrintCompute.customWantsCompute();
        }
        public static void main(String[] args) {
            Test test=new Test();
            IComputeFastory iComputeFastory=new ThinkPadFastoryImpl();//生产哪件产品由客户端决定
            test.buyCompute(iComputeFastory.createCompute());
        }
    }
  ```

### 抽象工厂模式：提供一个创建一系列相关的或互相依赖对象的接口，而无需指定它们具体的类。

组成：

1.多个抽象产品类
2.多个具体产品类
3.抽象工厂类-声明（一组）返回抽象产品的方法
4.具体工厂类-生成（一组）具体产品
工厂方法模式与抽象工厂模式基本类似，可以这么理解：当工厂只生产一个产品的时候，即为工厂方法模式；而工厂如果生产两个或以上的商品为抽象工厂模式。
优点：代码解耦，实现多个产品，很好的满足OCP原则，对复杂对象的生产灵活易扩展。
缺点：扩展产品族相当麻烦，而扩展产品族会违反OCP原则，因为要修改所有的工厂。由于抽象工厂模式时工厂方法模式的扩展，很笨重。

 ```java
    package JavaIDEAStudy;


    interface IPrintCompute{//抽象产品接口
    void customWantsCompute();
}
class MacImpl implements IPrintCompute{
    @Override
    public void customWantsCompute() {
        System.out.println("生产一台mac电脑");
    }
}
class ThinkPadImpl implements IPrintCompute{
    @Override
    public void customWantsCompute() {
        System.out.println("生产一台联想电脑");
    }
}
interface ComputeSystem{//抽象产品接口
        void createComputeSystem();
}
class MacOsSystemImpl implements ComputeSystem{
    @Override
    public void createComputeSystem() {
        System.out.println("这是苹果系统");
    }
}
class WindowsSystemImpl implements ComputeSystem{
    @Override
    public void createComputeSystem() {
        System.out.println("这是安卓系统");
    }
}
interface IComputeFastory{
        IPrintCompute createCompute();
        ComputeSystem createSystem();
}
class MacFastoryImpl implements IComputeFastory{
    @Override
    public IPrintCompute createCompute() {
        return new MacImpl();
    }

    @Override
    public ComputeSystem createSystem() {
        return new MacOsSystemImpl();
    }
}
class ThinkPadFastoryImpl implements IComputeFastory{
    @Override
    public IPrintCompute createCompute() {
        return new ThinkPadImpl();
    }

    @Override
    public ComputeSystem createSystem() {
        return new WindowsSystemImpl();
    }
}
    public class Test{
        public void buyCompute(IPrintCompute iPrintCompute){
            iPrintCompute.customWantsCompute();
        }
        public void getComputeSystem(ComputeSystem computeSystem){
            computeSystem.createComputeSystem();
        }
        public static void main(String[] args) {
            Test test=new Test();
            IComputeFastory iComputeFastory=new MacFastoryImpl();
            test.buyCompute(iComputeFastory.createCompute());
            ComputeSystem computeSystem=new WindowsSystemImpl();
            test.getComputeSystem(computeSystem);
        }
    }

 ```

简单工厂模式、工厂方法模式、抽象工厂模式总结：
简单工厂模式最大优点时工厂内有具体的逻辑去判断生成什么产品，将类的实例化交给了工厂，这样当我们需要什么产品只需要修改工厂的调用而不用修改客户端，对于客户端来说降低了与具体产品的依赖。
工厂方法模式时简单工厂的扩展，工厂方法模式将原先简单工厂中的实现那个类的逻辑判断交给了客户端，如果像添加功能只需要修改客户和添加具体的功能，不用去修改之前的类。
抽象工厂模式进一步扩展了工厂方法模式，它把原先的工厂方法模式中只能有一个抽象产品不能添加产品族的缺点克服了，抽象工厂模式不仅仅遵循了OCP原则，而且可以添加更多产品（抽象产品），具体工厂也不仅仅只是生成单一产品，而是生成一组产品，抽象工厂也是声明一组产品，对应扩展更加灵活，但是要是扩展族系会很笨重。