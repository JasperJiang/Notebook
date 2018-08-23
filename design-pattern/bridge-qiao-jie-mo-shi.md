# Bridge 桥接模式

桥接模式概念：**把抽象化与实现化解耦，使得二者可以独立变化。**

不懂，举例：每个公司都有**电话转接器**，每当客户打电话到总台，然后输入对应的分机号，就可以转接到对应工号的电话上了。这里的电话转接器就作为一个**桥**，起了桥接的作用，这就是桥接模式。

首先创建电话接口，声明接电话方法

```java
public interface Phone {
    void sayHello();
}
```

然后至少创建两个实现类

```java
public class Number1 implements Phone {
    @Override
    public void sayHello() {
        System.out.println("您好，这里是工号1001");
    }
}

public class Number2 implements Phone{
    @Override
    public void sayHello() {
        System.out.println("您好，这里是工号1002");
    }
}
```

然后我们就可以创建一个bridge桥，用来作为电话转接器，注意引用电话接口，用于维护转接的行为

```java
public class Bridge {
    //引用电话接口，用于维护桥接的具体行为
    private Phone phone;

    public Phone getPhone() {
        return phone;
    }

    public void setPhone(Phone phone) {
        this.phone = phone;
    }
    /* 桥接方法 */
    public void transfer(){
        System.out.println("正在为您转接...");
        phone.sayHello();
        System.out.println("");
    }
}
```

客户端

```java
Bridge bridge=new Bridge();//创建电话转接器

bridge.setPhone(new Number1());//接到电话，请求转接到工号1001

bridge.transfer();//进行转接

bridge.setPhone(new Number2());//接到电话，请求转接到工号1002

bridge.transfer();//进行转接
```

输出结果

```text
正在为您转接...
您好，这里是工号1001

正在为您转接...
您好，这里是工号1002
```

通过桥接的方式，利用桥bridge可以调用不同工号的方法，当然，也可以把桥也抽象出来，便于扩展不同的桥接方法。   
可以注意到，桥接模式的核心就是**抽象部分从原来的基础上分离开了，可以根据接口定义很多很多很多个独立的实现类而不影响整理的结构，实现部分也从原来的基础上分离开了，可以从业务角度去扩展独立的转接方法，不仅仅执行接口实现的方法。**

总结：   
1. 桥接模式通过同一个入口，可以加载到不同的类，继而可以调用该类对应的方法。数据库的驱动加载就是属于桥接模式。   
2. 桥接模适用于项目中**同一个接口有很多个实现**、或者**相同接口之间有很多种调用关系**的情况，如果仅仅的使用继承去实现，可能会造成类爆炸，对于扩展和维护都不太友好。

