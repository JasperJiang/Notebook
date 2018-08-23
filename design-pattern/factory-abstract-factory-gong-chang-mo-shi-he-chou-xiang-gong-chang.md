# Factory &  Abstract Factory 工厂模式和抽象工厂

工厂方法模式和抽象工厂模式是两个不同的设计模式，但个人认为两种模式放到一起会更容易理解他们。

工厂方法模式的概念：定义一个用于创建对象的接口，让子类决定实例化哪一个类，不懂？接着往下看。

常见的工厂方法模式有几种：**简单工厂、多方法工厂，工厂方法**

举例：通过工厂方法模式创建并获取一个因特尔CPU对象，假设这个CPU对象的创建过程很复杂。

**简单（静态）工厂：工厂静态声明了各个产品的创建接口和参数，直接调用接口并使用特定的参数即可获取特定的产品对象。**

首先创建一个CPU抽象类（基类）

```java
public abstract class CPU {
    /**
     * 展示CPU的品牌
     */
    public abstract void showBrand();
}
```

根据CPU的品牌，创建两个实现类

```java
public class IntelCPU extends CPU {
    /**
     * 输出因特尔 CPU的品牌
     */
    @Override
    public void showBrand() {
        System.out.println("It's IntelCPU.");
    }
}
```

```java
public class AMDCPU extends CPU {
    /**
     * 输出AMD CPU的品牌
     */
    @Override
    public void showBrand() {
        System.out.println("It's AMDCPU.");
    }

}
```

CPU类有了，接着就可以创建简单工厂类，这个工厂可以生产两种品牌的CPU

```java
public class SimpleCPUFactory {
    public static final int TYPE_INTEL = 1;// 因特尔处理器
    public static final int TYPE_AMD = 2;// AMD处理器

    public static CPU createCPU(int type) {
        switch (type) {
        case TYPE_INTEL:
            System.out.println("=====开始创建IntelCPU=====");
            System.out.println("=====此处省略很多个步骤...=====");
            System.out.println("=====成功创建IntelCPU=====");
            return new IntelCPU();
        case TYPE_AMD:
            System.out.println("=====开始创建AMDCPU=====");
            System.out.println("=====此处省略很多个步骤...=====");
            System.out.println("=====成功创建AMDCPU=====");
            return new AMDCPU();
        default:
            return null;
        }
    }
}
```

于是，我们就可以通过工厂提供的静态方法方便的创建并获取因特尔CPU对象了

```java
// 通过工厂创建并获取因特尔CPU
CPU cpu = SimpleCPUFactory.createCPU(SimpleCPUFactory.TYPE_INTEL);
// 输出CPU的品牌
cpu.showBrand();
```

输出结果

```text
=====开始创建IntelCPU=====
=====此处省略很多个步骤...=====
=====成功创建IntelCPU=====
It's IntelCPU.
```

简单工厂很准确的体现了工厂方法模式的核心概念：**当某个对象的创建过程很复杂的时候，我们就可以定义一个工厂类，约定好创建对象的接口和参数，对象的创建过程由工厂类内部完成，客户端不需要知道对象的创建细节，只需要根据接口和参数，便可根据自己的需要来创建并获取不同的产品对象。**   
特点：简单、清晰

缺陷：扩展性较差，如果需要添加产品，首先要创建产品类，同时需要修改工厂类源码。

**基于反射的简单工厂：利用反射的机制，动态创建产品对象。**   
修改工厂类的创建CPU接口入参为Class，实现里面通过Class.forName反射创建对象，客户端调用时入参变为对象的类。

```java
public class ReflectCPUFactory {
    //使用泛型定义入参，通过反射Class.forName(cls.getName()).newInstance()创建对象
    public static <T extends CPU> T createCPU(Class<T> cls) {
        T cpu = null;
        try {
            System.out.println("=====开始创建CPU=====");
            System.out.println("=====此处省略很多个步骤...=====");
            System.out.println("=====成功创建CPU=====");
            cpu = (T) Class.forName(cls.getName()).newInstance();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return cpu;
    }
}
```

特点：解决上面静态简单工厂存在的一个扩展性缺陷，添加产品的时候只需要创建对应的类，不需要修改工厂类。slf4j的日志工厂就是采用的这种方式。

缺陷：所有产品的创建过程都一样了，不支持多参数。如果不同的产品具有不同的属性值时，不能满足。

**多方法工厂：提供多个创建方法，用于创建不同的产品对象**   
工厂类为每个不同的产品提供创建方法

```java
public class ManyMethodsCPUFactory {
    /**
     * 创建并获取Intel CPU
     */
    public static CPU createIntelCPU() {
        System.out.println("=====开始创建IntelCPU=====");
        System.out.println("=====此处省略很多个步骤...=====");
        System.out.println("=====成功创建IntelCPU=====");
        return new IntelCPU();
    }

    /**
     * 创建并获取AMD CPU
     */
    public static CPU createAMDCPU() {
        System.out.println("=====开始创建AMDCPU=====");
        System.out.println("=====此处省略很多个步骤...=====");
        System.out.println("=====成功创建AMDCPU=====");
        return new AMDCPU();
    }
}
```

特点：为每个产品或每种产品提供对应的创建方法，需要哪个对象就调用哪个方法，需要什么属性就传什么属性，使用方便。

缺陷：同样的扩展性差，添加产品的时候，除了添加产品类，还需要修改源码，不过好在只需要在工厂类源码基础上添加方法，而不用修改工厂类以前的代码

**工厂方法：不仅仅是产品需要抽象，工厂层面也需要抽象一个基类工厂。因特尔CPU工厂只能生产因特尔CPU**   
创建一个抽象工厂基类

```java
public abstract class AbstractCPUFactory {
    // 定义抽象方法，由子工厂类负责具体的实现逻辑
    public abstract CPU createCPU();
}
```

接着分别创建两种CPU的工厂子类

```java
public class IntelCPUFactory extends AbstractCPUFactory {
    @Override
    public CPU createCPU() {
        System.out.println("=====开始创建IntelCPU=====");
        System.out.println("=====此处省略很多个步骤...=====");
        System.out.println("=====成功创建IntelCPU=====");
        return new IntelCPU();
    }
}
```

```java
public class AMDCPUFactory extends AbstractCPUFactory {
    @Override
    public CPU createCPU() {
        System.out.println("=====开始创建AMDCPU=====");
        System.out.println("=====此处省略很多个步骤...=====");
        System.out.println("=====成功创建AMDCPU=====");
        return new AMDCPU();
    }
}
```

由于工厂也是抽象的，因此需要先new一个工厂子类，然后再获取CPU对象

```java
public static void main(String[] args) {
        // 创建专门生产因特尔CPU的工厂
        AbstractCPUFactory cpuFactory = new IntelCPUFactory();
        // 根据工厂创建并获取CPU对象
        CPU cpu = cpuFactory.createCPU();

        cpu.showBrand();
}
```

特点：产品类和工厂类都抽象出来，更符合设计模式的单一职责和开闭原则，提高了扩展性

缺陷：每增加一个不同种类的产品就需要额外再添加一个工厂类，在产品种类很多的情况下十分不利于维护

以上所有工厂方法模式都只针对**单产品**，每次都只能创建并获取到单个产品对象。抽象工厂模式则是针对**多产品**，每个工厂能提供多个产品对象的接口，而不需要指定参数。

抽象工厂模式概念：为创建一组相关或相互依赖的对象提供一个接口，而且无须指定它们的具体类，还是不懂？往下看。   
**抽象工厂模式：在工厂方法模式的基础上，每个工厂提供一系列相关或互相依赖对象的接口**   
举例：通过抽象工厂模式创建并获取因特尔CPU对象和与之匹配的主板、内存，假设这些对象的创建过程都很复杂。

已经有了CPU对象类，需要再创建主板、内存的对象类

```java
public abstract class Mainboard {
    /**
     * 展示主板的信息
     */
    public abstract void showMessage();
}

public class IntelMainboard extends Mainboard {
    /**
     * 输出主板的信息
     */
    @Override
    public void showMessage() {
        System.out.println("我是主板A，我能匹配Intel CPU");
    }
}

public class AMDMainboard extends Mainboard {
    /**
     * 输出主板的信息
     */
    @Override
    public void showMessage() {
        System.out.println("我是主板B，我能匹配AMD CPU");
    }
}
```

```java
public abstract class Memory {
    /**
     * 展示内存的信息
     */
    public abstract void showMessage();
}

public class IntelMemory extends Memory {
    /**
     * 输出内存的信息
     */
    @Override
    public void showMessage() {
        System.out.println("我是内存A，我能匹配主板A");
    }
}

public class AMDMemory extends Memory {
    /**
     * 输出内存的信息
     */
    @Override
    public void showMessage() {
        System.out.println("我是内存B，我能匹配主板B");
    }
}
```

工厂类也是抽象的，与工厂方法模式不同的是，每个工厂提供一组相互依赖的对象创建方法，可以创建并获取CPU、主板和内存

```java
public abstract class AbstractFactory {
    // 定义抽象方法，由子工厂类负责具体的实现逻辑
    public abstract CPU createCPU();

    public abstract Mainboard createMainBoard();

    public abstract Memory createMemory();
}
```

此时因特尔工厂就应该只能生产因特尔CPU和与因特尔cpu匹配的主板、内存，AMD同理。

```java
public class IntelFactory extends AbstractFactory {

    @Override
    public CPU createCPU() {
        System.out.println("=====开始创建IntelCPU=====");
        System.out.println("=====此处省略很多个步骤...=====");
        System.out.println("=====成功创建IntelCPU=====");
        return new IntelCPU();
    }

    @Override
    public Mainboard createMainBoard() {
        System.out.println("=====开始创建与IntelCPU匹配的主板A=====");
        System.out.println("=====此处省略很多个步骤...=====");
        System.out.println("=====成功创建主板A=====");
        return new IntelMainboard();
    }

    @Override
    public Memory createMemory() {
        System.out.println("=====开始创建与主板A匹配的内存A=====");
        System.out.println("=====此处省略很多个步骤...=====");
        System.out.println("=====成功创建内存A=====");
        return new IntelMemory();
    }

}
```

```java
public class AMDFactory extends AbstractFactory {

    @Override
    public CPU createCPU() {
        System.out.println("=====开始创建AMD CPU=====");
        System.out.println("=====此处省略很多个步骤...=====");
        System.out.println("=====成功创建AMD CPU=====");
        return new AMDCPU();
    }

    @Override
    public Mainboard createMainBoard() {
        System.out.println("=====开始创建与AMD CPU匹配的主板B=====");
        System.out.println("=====此处省略很多个步骤...=====");
        System.out.println("=====成功创建主板B=====");
        return new AMDMainboard();
    }

    @Override
    public Memory createMemory() {
        System.out.println("=====开始创建与主板B匹配的内存B=====");
        System.out.println("=====此处省略很多个步骤...=====");
        System.out.println("=====成功创建内存B=====");
        return new AMDMemory();
    }

}
```

客户端想要创建并获取因特尔CPU对象和与之匹配的主板、内存，只需要new一个因特尔工厂，就可以拿到所有需要的对象，而不需要知道每个对象的创建细节和匹配度。

```java
// 创建专门生产因特尔CPU相关的工厂
AbstractFactory intelFactory = new IntelFactory();
// 根据工厂创建并获取CPU对象
CPU cpu = intelFactory.createCPU();
// 根据工厂创建并获取主板对象
Mainboard mainboard = intelFactory.createMainBoard();
// 根据工厂创建并获取内存对象
Memory memory = intelFactory.createMemory();

cpu.showBrand();
mainboard.showMessage();
memory.showMessage();
```

输出结果

```text
=====开始创建IntelCPU=====
=====此处省略很多个步骤...=====
=====成功创建IntelCPU=====
=====开始创建与IntelCPU匹配的主板A=====
=====此处省略很多个步骤...=====
=====成功创建主板A=====
=====开始创建与主板A匹配的内存A=====
=====此处省略很多个步骤...=====
=====成功创建内存A=====
It's IntelCPU.
我是主板A，我能匹配Intel CPU
我是内存A，我能匹配主板A
```

抽象工厂模式的核心就是**有一组（两个以上）相关或相互依赖的产品时，由工厂负责维护各个产品的创建细节和依赖关系，客户端只需要知道该组产品的工厂实现类，就可以获取到所有相关的产品对象。**

特点：一个工厂维护一组产品，适合多个产品关联性或依赖性较强的场景。就像例子中的CPU和主板的匹配度，主板和内存条的匹配度。

缺陷：粒度变大，每个工厂从维护单个产品到维护一组产品，维护成本极高。如果新增产品或修改产品关联关系，几乎不可能完成。

总结：   
1. 不管是工厂方法模式还是抽象工厂模式，都只适用于**创建过程复杂的对象**。   
2. 两种模式的核心都大同小异，对象的复杂创建过程由工厂维护，只提供统一的接口，方便客户端创建并获取对象，不同的是工厂方法模式针对单产品，抽象工厂模式针对一组有相关或依赖的产品。   
3. 每种方式都各有利弊，个人认为，在变化快速的今天，抽象工厂模式只适合以API的形式出现，而开发过程中推荐多方法工厂。

