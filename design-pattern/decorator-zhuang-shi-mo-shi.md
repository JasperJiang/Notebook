# Decorator 装饰模式

装饰模式概念：以对客户端透明的方式扩展对象的功能，是继承关系的一个替代方案。

例子：还是以打官司为例子，不管过程和结果怎么样，我只是签字而已，可是对外我不能跟人说我只是签个字就赢了这个官司，此时我就需要把这全部的事迹请包装公司包装一下，然后由包装公司帮我对外说我是怎么怎么做才赢下的官司。

还是先创建接口

```java
public interface Sourceable {
    /* 打官司 */
    public void lawsuit();
}
```

接口实现类，我只能签字，接受结果

```java
public class RealSource implements Sourceable {
    @Override
    public void lawsuit() {
        System.out.println("签字，接受结果");
    }
}
```

创建装饰类，注意装饰类需要引入和被装饰对象同一个接口，同时能够动态设置被装饰对象

```java
public class Decorator implements Sourceable {
    /* 和被装饰对象同样的接口 */
    Sourceable sourceable;

    public void setSourceable(Sourceable sourceable) {
        this.sourceable = sourceable;
    }

    /* 对原方法进行包装 */
    @Override
    public void lawsuit() {
        doSomeThing();
        sourceable.lawsuit();
    }

    private void doSomeThing() {
        System.out.println("经过..........，最终赢下的官司");
    }
}
```

客户端

```java
Sourceable sourceable=new RealSource();//创建被装饰对象

Decorator decorator=new Decorator();//创建装饰者对象

decorator.setSourceable(sourceable);//将被装饰对象放到装饰者里面进行装饰

decorator.lawsuit();//由装饰者对象对外提供服务
```

输出结果

```text
经过..........，最终赢下的官司
签字，接受结果
```

装饰模式的核心就是**创建一个装饰类，能够动态的包装原有接口下的类，并在保持类方法签名完整性的前提下，提供了额外的功能。**

可能很多人不理解，装饰模式和代理模式的区别是什么，他们都能在方法的前后加一些功能，这里我言简意赅的说明一下：

* **代理模式是静态的对某一个具体的实现类的方法进行扩展，侧重于该方法的控制。**
* **装饰模式对方法进行扩展后，能够够动态的注入被装饰的对象，对一个接口下的所有实现子类都有效，侧重于功能的增强。**

同时很多人会问，那装饰模式和桥接模式不一样的吗，都能够动态的加载对象，然后执行相应的方法：

* **桥接模式侧重于将抽象和实现分离，由桥自己定义的方法完成功能的扩展。不需要实现原接口。**
* **装饰模式侧重于功能的增强，是在原接口方法的基础上增加功能，因此会实现原接口。**

总结：   
1. 通常我们在扩展功能的时候，都是采用继承的方式，这就导致子类越来越多，不便于管理和维护。如果用装饰模式，添加功能只需要添加一个装饰者，由装饰者负责功能的添加。   
2. 装饰模式相对于继承有更好的灵活性，尤其是两级以上的子类继承。   
3. 装饰模式也**只适用于接口与原有的实现都已经确定不修改的情况**。   
4. 装饰模式是java I/O库的基本设计模式。而具体在I/O库中怎么应用的，下来有时间再整理吧。

