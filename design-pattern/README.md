# Design Pattern

#### **设计模式遵循的原则有6个**

**1、开闭原则（Open Close Principle）**

　　**对扩展开放，对修改关闭**。

**2、里氏代换原则（Liskov Substitution Principle）**

　　只有当衍生类可以替换掉基类，软件单位的功能不受到影响时，基类才能真正被复用，而衍生类也能够在基类的基础上增加新的行为。

**3、依赖倒转原则（Dependence Inversion Principle）**

　　这个是开闭原则的基础，**对接口编程**，依赖于抽象而不依赖于具体。

**4、接口隔离原则（Interface Segregation Principle）**

　　使用多个隔离的借口来降低耦合度。

**5、迪米特法则（最少知道原则）（Demeter Principle）**

　　一个实体应当尽量少的与其他实体之间发生相互作用，使得系统功能模块相对独立。

**6、合成复用原则（Composite Reuse Principle）**

　　原则是尽量使用合成/聚合的方式，而不是使用继承。继承实际上破坏了类的封装性，超类的方法可能会被子类修改。

### 针对借口编程，而不是针对实现编程

### 最少知识原则：只和你的密友谈话

**不采用**这个原则

```java
public float getTemp(){
    Thermometer thermometer = station.getThermometer();
    return thermometer.getTemperature();
}
```

这里，我们从天气站取得了温度计对象，然后再从温度计对象取得温度

**采用**这个原则

```java
public float getTemp(){
    return station.getTemperature();
}
```

应用此原则时，我们在气象站中加进一个方法，用来向温度计请求温度。这可以减少我们所依赖的类的数目。



