# Facade 外观模式

外观模式概念：隐藏系统的复杂性，并向客户端提供了一个客户端可以访问系统的接口

很好理解的例子：电脑买好了，只需要按“开机”键就开机了，但对系统来讲开机过程其实挺复杂的，这个开机键就是外观模式的接口，而系统内部的复杂操作不需要外界知道。

开机过程需要启动主板、CPU、内存

```java
/** 主板类 */
public class Mainboard {
    public void startMainboard() {
        System.out.println("正在启动主板");
    }
}

/** CPU类 */
public class CPU {
    public void startCPU() {
        System.out.println("正在启动CPU");
    }
}

/** 内存类 */
public class Memory {
    public void startMemory() {
        System.out.println("正在启动内存");
    }
}
```

然后创建外观类，对外提供一键开机功能

```java
public class ComputerFacade {
    private CPU cpu;
    private Mainboard mainboard;
    private Memory memory;

    public ComputerFacade() {
        cpu = new CPU();
        mainboard = new Mainboard();
        memory = new Memory();
    }

    /** 一键开机 */
    public void startComputer() {
        mainboard.startMainboard();
        cpu.startCPU();
        memory.startMemory();
        System.out.println("...开机完成...");
    }
}
```

客户端

```java
ComputerFacade facade=new ComputerFacade();
facade.startComputer();//一键开机
```

输出结果

```text
正在启动主板
正在启动CPU
正在启动内存
...开机完成...
```

外观模式的核心就是**创建一个外观类，对外隐藏复杂的内部操作，同时提供一个高层的入口供外部访问，简化了客户端的操作**。

总结：   
1. 外观模式比较简单，主要是为复杂的模块或子系统提供外界访问的简单入口，因此适用于**子模块相对独立的复杂操作**。   
2. 外观模式让各个子模块相互**解耦，增加了系统的灵活性**。

