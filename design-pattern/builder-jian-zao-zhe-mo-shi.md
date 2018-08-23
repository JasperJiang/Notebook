# Builder 建造者模式

建造者模式概念：将一个复杂的对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。不懂？换个解释： 

  
**想玩吃鸡，需要一个性能好一点的电脑主机，可是电脑主机有CPU、主板、内存等配件，如果每个配件都单独购买，就要考虑CPU与主板之间有匹配问题，主板与内存条之间也有匹配问题，作为小白的我不知道怎么办。所以商家直接给出了几个主机套装方案，我只需要选择一个套装方案，付钱，然后等装机师傅组装好就能得到一个电脑主机。这里的几个套装方案就是对应几个“建造者builder”。还没理解？看下面**

首先定义一个电脑主机类。按理说CPU、主板、内存条也是对象，体现每个配件的复杂，这里为了方便理解所以用String，也没提供getset方法

```java
public class Mainframe {
    // CPU
    public String cpu;
    // 主板
    public String mainboard;
    // 内存条
    public String memory;
}
```



接着抽象一个建造者的基类

```java
public abstract class MainframeBuilder {
    /** 组装CPU */
    public abstract void buildCPU();
    /** 组装主板 */
    public abstract void buildMainBoard();
    /** 组装内存条 */
    public abstract void buildMemory();
    /** 获取产品 */
    public abstract Mainframe getMainframe();
}
```

然后创建两种方案，即创建两个建造者实现类

```java
public class IntelMainframeBuilder extends MainframeBuilder {
    private Mainframe mainframe = new Mainframe();
    @Override
    public void buildCPU() {
        mainframe.cpu = "Intel cpu";
    }
    @Override
    public void buildMainBoard() {
        mainframe.mainboard = "与Intel cpu 匹配的主板A";
    }
    @Override
    public void buildMemory() {
        mainframe.memory = "与主板A匹配的内存条A";
    }
    @Override
    public Mainframe getMainframe() {
        return mainframe;
    }
}

public class AMDMainframeBuilder extends MainframeBuilder {
    private Mainframe mainframe = new Mainframe();
    @Override
    public void buildCPU() {
        mainframe.cpu = "AMD cpu";
    }
    @Override
    public void buildMainBoard() {
        mainframe.mainboard = "与AMD cpu 匹配的主板B";
    }
    @Override
    public void buildMemory() {
        mainframe.memory = "与主板B匹配的内存条B";
    }
    @Override
    public Mainframe getMainframe() {
        return mainframe;
    }
}
```

最后需要一个导演类，专门负责把方案变为产品

```java
public class Director {
    /** 当前需要创建产品的建造者 */
    private MainframeBuilder builder;

    /**
     * 构造方法，设置当前需要创建产品的建造者
     */
    public Director(MainframeBuilder builder) {
        this.builder = builder;
    }

    /**
     * 产品建造方法，指导建造者创建产品
     */
    public void construct() {
        builder.buildCPU();
        builder.buildMainBoard();
        builder.buildMemory();
    }
}
```



客户端调用

```java
// 选择方案，想要一台有因特尔cpu的主机
MainframeBuilder builder = new IntelMainframeBuilder();
// 付钱，把方案告诉导演
Director director = new Director(builder);
// 导演根据方案，指导完成主机组装
director.construct();
// 拿到主机对象
Mainframe mainframe = builder.getMainframe();
System.out.println(mainframe.cpu);
System.out.println(mainframe.mainboard);
System.out.println(mainframe.memory);

```

输出结果

```text
Intel cpu
与Intel cpu 匹配的主板A
与主板A匹配的内存条A
```

建造者模式的核心就是在**针对某个复杂对象的创建时（复杂指的是对象里面的属性有很多不同的组合），提供许多个builder（方案），客户端只需要使用不同的builder，就可以用同样的方式获取到不同的对象**

有人可能会问，建造者模式和抽象工厂模式不都一个道理吗？都是可以创建并获取有关联的CPU、主板和内存。   
答：看似一样，其实不然，抽象工厂模式最终得到的产品对象是多个。而建造者模式最终得到的产品对象只有一个，只是这个产品对象里面的属性可能比较复杂或者存在依赖关系。也可以理解为建造者模式比抽象工厂模式要高一层。

总结：   
1. 建造者模式适用于创建**产品对象属性创建复杂或者存在依赖关系的复杂产品**。而客户端只需要选择一个builder，使用同样的方式就可以获取到这个builder对应的产品。   
2. 符合开闭原则，扩展性较高，如果新添某一种方案，只需要新添一个builder类即可，如果新添产品属性，或产品建造方式改变的时候，只需要在导演类中添加一个新的建造方法，满足该种产品建造

