# Proxy 代理模式

代理模式概念：给某一个对象提供一个代理对象，并由代理对象控制对原对象的引用。

简单的例子：我请代理律师帮忙打官司，他知道法律相关的东西，而我不了解法律，我只知道签字，如果叫我去打官司我肯定打不赢，所以我叫代理律师代替我去打官司。

创建接口

```java
public interface Sourceable {
    /* 打官司 */
    public void lawsuit();
}
```

接口实现类，如果我来打官司的话，我只能签字，接受结果

```java
public class RealSource implements Sourceable {
    @Override
    public void lawsuit() {
        System.out.println("签字，接受结果");
    }
}
```

然后创建一个代理类，他能帮我打官司

```java
public class Proxy implements Sourceable {
    RealSource realSource = new RealSource();

    @Override
    public void lawsuit() {
        System.out.println("我是代理律师，我帮你打赢官司");//做一些我做不到的事情
        realSource.lawsuit();
    }
}
```

客户端

```java
Sourceable sourceable=new Proxy();
sourceable.lawsuit();
```

输出结果

```text
我是代理律师，我帮你打赢官司
签字，接受结果
```

代理模式的核心就是**当现有对象不能满足需要的时候，就创建一个代理类，代理类持有现有的对象，在控制现有对象访问的同时，能做一些其他的事情，以此来满足我们的需要。**

总结：   
1. 代理模式很好的体现了设计模式的开闭原则，拥有较高的扩展性。当我有新的需求时，可以创建一个新的代理类，来满足需求，而不是在我的实现类里面去修改。同时即使我修改了实现类，对现有的代理类也没有任何影响。   
2. 代理模式的职责单一，结构很清晰，能提高系统的可读性和可维护性。   
3. 代理模式**只适用于接口与原有的实现都已经确定不修改的情况**。spring就是利用的代理模式，为每个bean都生成代理，我们实际请求bean都是代理对象。

