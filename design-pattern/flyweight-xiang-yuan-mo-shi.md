# Flyweight 享元模式

享元模式概念：使用共享对象进而有效地支持大量的细粒度的对象

首先理解两个概念：内部状态、外部状态

* 内部状态：在享元对象内部不随外界环境改变而改变的共享部分。
* 外部状态：随着环境的改变而改变，不能够共享的状态就是外部状态。

都不懂，很简单：现在有一个画图软件，能画出各种颜色的圆形。我们就可以把圆形作为享元对象，颜色就作为内部状态是可以共享的。

首先创建享元类，圆形

```java
public class Circle {
    /* 颜色 */
    String color;

    public Circle(String color) {
        this.color = color;
    }

    public void draw() {
        System.out.println("画出 " + color + " 的圆形");
    }
}
```

接着创建共享池，通常和工厂模式共用，如果共享池里面存在需要的对象，就从共享池里面取，否则就创建对象，同时放到共享池里面。

```java
public class CircleFactory {
    /** 通常用hashMap作为享元池存储对象 */
    static final HashMap<String, Circle> circleMap = new HashMap<>();

    public static Circle getCircle(String color) {
        Circle circle = (Circle) circleMap.get(color);

        if (circle == null) {
            circle = new Circle(color);
            circleMap.put(color, circle);
        }

        return circle;
    }
}
```

客户端

```java
Circle circle1 = CircleFactory.getCircle("红色");
circle1.draw();

Circle circle2 = CircleFactory.getCircle("白色");
circle2.draw();

Circle circle3 = CircleFactory.getCircle("蓝色");
circle3.draw();

Circle circle4 = CircleFactory.getCircle("红色");
circle4.draw();

Circle circle5 = CircleFactory.getCircle("白色");
circle5.draw();

System.out.println("共享池里面共有" + CircleFactory.circleMap.size() + "个对象");
```

输出结果

```text
画出 红色 的圆形
画出 白色 的圆形
画出 蓝色 的圆形
画出 红色 的圆形
画出 白色 的圆形
共享池里面共有3个对象
```

可以看到，我们共画了5个圆形，可工厂其实只创建了3个对象，因为相同颜色的圆形已经在共享池里面存在了。   
享元模式的核心就是**利用hashMap作为共享池，把可以共享的对象都放到里面，之后在使用到相同的对象时就不需要重新创建对象，只需要从共享池里面获取就可以了，极大的减少了内存的占用和资源的消耗。**

总结：   
1. 享元模式适合系统中存在大量的对象，并且这些对象存在一定的相同或相似的元素（内部状态）   
2. 享元模式通常和工厂模式一起使用，大大减少对象的创建，降低系统的内存，使效率提高。   
3. 享元模式最主要的特点是存在一个共享池，java中的String类型和jdbc数据库连接池都是采用的享元模式。

