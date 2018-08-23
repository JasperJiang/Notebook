# Prototype 原型模式

概念：用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象。

关键词：**拷贝**。获取对象不是通过new（类实例化）的方式，而是通过clone（拷贝）的方式

最主要的优点：**对象创建速度快，显著提升性能**

适用场景：直接创建对象需要花费大量的资源，同时该对象需要大量重复的创建。

例子：10000人的公司需要统一使用办公电脑，硬件环境每个人都一样，软件环境则根据每个人的需要自行安装。假设每个电脑的创建需要花费很多的资源，此时就可以采用原型模式，把刚装好系统的裸机作为**原型**，通过拷贝的方式快速的为每个人创建一个电脑。

声明原型类，实现Cloneable接口才能使用clone方法

```java
public class Computer implements Cloneable {
    /* 硬件 */
    private String hardware;
    /* 软件 */
    private String software;

    protected Computer clone() throws CloneNotSupportedException {
        this.hardware = "cpu四核+内存16G+存储256G+1T";
        return (Computer) super.clone();
    }

    public String getHardware() {
        return hardware;
    }

    public void setHardware(String hardware) {
        this.hardware = hardware;
    }

    public String getSoftware() {
        return software;
    }

    public void setSoftware(String software) {
        this.software = software;
    }
}
```

客户端

```java
public static void main(String[] args) throws CloneNotSupportedException {
    Computer prototype = new Computer();//原型只new一次

    Computer computer1 = prototype.clone();//克隆
    System.out.println("computer1-hardware:" + computer1.getHardware());
    System.out.println("computer1-software:" + computer1.getSoftware());

    Computer computer2 = prototype.clone();//克隆
    System.out.println("computer2-hardware:" + computer2.getHardware());
    System.out.println("computer2-software:" + computer2.getSoftware());
}
```

输出结果

```text
computer1-hardware:cpu四核+内存16G+存储256G+1T
computer1-software:null
computer2-hardware:cpu四核+内存16G+存储256G+1T
computer2-software:null
```

原型模式的核心就是**把对象生成的方式从new变成了clone，通过复制拷贝而不通过构造函数去实例化对象。因此只适用于直接创建对象代价比较大，同时又需要多次重复创建的情况。也正是因此，原型模式通常是在资源优化的时候才会考虑使用。**

补充一点：   
浅复制：将一个对象复制后，基本数据类型的变量都会重新创建，而引用类型，指向的还是原对象所指向的。

深复制：将一个对象复制后，不论是基本数据类型还有引用类型，都是重新创建的。简单来说，就是深复制进行了完全彻底的复制，而浅复制不彻底。

例子中只是简单的实现了super.clone\(\)，属于浅复制，String属于引用类型，因此computer1和computer2的hardware是同一个引用

```java
computer1.getHardware() == computer2.getHardware();//比较结果为true
```

如果要实现深复制，可以采用二进制流的方式完全复制对象。

```java
/* 深复制 */
public Object deepClone() throws IOException, ClassNotFoundException {
    this.hardware = "cpu四核+内存16G+存储256G+1T";

    /* 写入当前对象的二进制流 */
    ByteArrayOutputStream bos = new ByteArrayOutputStream();
    ObjectOutputStream oos = new ObjectOutputStream(bos);
    oos.writeObject(this);

    /* 读出二进制流产生的新对象 */
    ByteArrayInputStream bis = new ByteArrayInputStream(bos.toByteArray());
    ObjectInputStream ois = new ObjectInputStream(bis);
    return ois.readObject();
}
```

使用深复制deepClone（）克隆出的对象，引用类型的比较结果则为false，为完全不同的两个对象。

总结：   
1. 原型模式只适用于直接创建对象需要花费大量的资源，同时该对象需要大量重复的创建的场景。   
2. **原型模式主要作用是提升性能，如果项目环境对性能要求较高的情况，同时刚好适合原型模式的使用场景，才建议考虑原型模式。**   
3. 原型类也可以抽象，有助于后期的扩展，这里展示的是直接在原型基础上复制。可根据实际情况而定。

