# Singleton 单例模式

### 概念：

保证一个类只存在一个实例，并将这个实例供整个系统访问。

常见的单例模式有五种：**饿汉式、懒汉式、双重检查锁、静态内部类、枚举类型**

**饿汉式：这个对象很饿、很急，程序加载就创建实例。关键词：static**

```java
public static class Singleton {  
    //定义并静态初始化一个实例，只供内部调用  
    private static final Singleton instance = new Singleton(); 
    //提供一个供外部访问单例对象的静态方法，可以直接访问单例对象  
    public static Singleton getInstance() {  
        return instance;  
    }  
    private Singleton(){}  
}  
```

饿汉式单例模式采取**空间换时间**的方式，不管对象是否被引用，都会创建这个实例。

**懒汉式：这个对象很懒、不急，第一次调用时才创建实例，但考虑到线程安全，必须加上同步关键字才能保证只实例化一次。关键词：synchronized**

```java
public class Singleton {  
    private static Singleton instance=null;  
    //为了保证线程安全，因此必须加上synchronized同步关键字
    public static synchronized Singleton getInstance() {  
        if(instance==null) {  
               instance=new Singleton();  
        }  
        return instance;  
    }  
    private Singleton(){}  
}
```

懒汉式单例模式则采取了**时间换空间**的方式，第一次调用这个对象的时候才会创建对象实例，但同步关键字导致每次调用单例对象都是线程同步的，影响程序性能，不推荐使用。

**双重检查锁形式：在懒汉式的基础上，只在创建实例的时候加锁，同时为了保证实例的唯一，创建实例的时候也需要进行一次判断。关键词：两个if**

```java
public class Singleton {
    private volatile static Singleton instance = null;

    public static Singleton getInstance() {
        if(instance == null) {
            //同步块，线程安全的创建实例
            synchronized (Singleton.class) {
                //再次检查实例是否存在，如果不存在才真正的创建实例
                if(instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }

    private Singleton(){}
}
```

双重检查锁形式只有**在第一次引用的时候才会线程同步去创建实例**，比懒汉式每次引用都同步性能提高了很多，但是每次引用的时候都会判断对象是否为空，有强迫症的开发人也不推荐使用。

**静态内部类：基于饿汉式单例模式，不同的是在单例类中声明一个静态内部类，在这个静态内部类中去静态实例化单例对象，通过这个静态内部类来获取单例对象。关键词：static class**

```java
public class Singleton {
    //静态内部类，该内部类的实例与外部类的实例没有绑定关系，而且只有被调用到时才会装载
    private static class SingletonHolder {
        //静态初始化器，由JVM来保证线程安全
        private static Singleton instance = new Singleton();
    }

    public static Singleton getInstance() {
        //通过访问内部类的成员变量来获取单例对象。
        return SingletonHolder.instance;
    }

    private Singleton(){}
}
```

静态内部类形式巧妙利用内部类的机制，在内部类中静态实例化单例对象，在启动加载外部的单例类时是不会实例化内部类的，因此也不会实例化单例对象，当**真正调用到内部类的时候才会加载内部类**，此时才会实例化单例对象。而每次获取单例对象都是通过静态类获取的，程序的性能不受影响。

**枚举类型：以枚举类型声明单例类。关键词：enum**

```java
public enum Singleton {
    singleton
}
```

这里的’singleton’就是单例类的一个实例，直接用Singleton.singleton就能拿到单例对象实例了。没错，枚举类就是这么简洁，而且无偿地提供了序列化机制，并由JVM从根本上提供保障，绝对实现实例唯一和线程安全，是更简洁、高效、安全的实现单例的方式。

总结：   
懒汉式和双重检查锁存在一定程度的性能浪费，因此不推荐   
饿汉式不能实现延迟加载，但是实现方法最基本最方便理解   
静态内部类的方式能实现延迟加载   
枚举方式最简洁安全高效   
在实际环境中，饿汉式、静态内部类和枚举方式都可以使用，根据实际的情况选择。

