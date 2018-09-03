# Serializable  序列化和反序列化

### 1.序列化和反序列化的概念

序列化：把对象转换为字节序列的过程称为对象的序列化。 

反序列化：把字节序列恢复为对象的过程称为对象的反序列化。

上面是专业的解释，现在来点通俗的解释。在代码运行的时候，我们可以看到很多的对象\(debug过的都造吧\)， 可以是一个，也可以是一类对象的集合，很多的对象数据，这些数据中， 有些信息我们想让他持久的保存起来，那么这个序列化。 就是把内存里面的这些对象给变成一连串的字节描述的过程。 常见的就是变成文件 我不序列化也可以保存文件啥的呀，有什么影响呢？我也是这么问的。

### 2.什么情况下需要序列化 

当你想把的内存中的对象状态保存到一个文件中或者数据库中时候；

当你想用套接字在网络上传送对象的时候；

当你想通过RMI传输对象的时候；

### 3.如何实现序列化

实现Serializable接口即可

上面这些理论都比较简单，下面实际代码看看这个序列化到底能干啥，以及会产生的bug问题。先上对象代码，飞猪.java

```java
package com.lxk.model;  
  
import java.io.Serializable;  
  
/** 
 * @author lxk on 2017/11/1 
 */  
public class FlyPig implements Serializable {  
    //private static final long serialVersionUID = 1L;  
    private static String AGE = "269";  
    private String name;  
    private String color;  
    transient private String car;  
  
    //private String addTip;  
  
    public String getName() {  
        return name;  
    }  
  
    public void setName(String name) {  
        this.name = name;  
    }  
  
    public String getColor() {  
        return color;  
    }  
  
    public void setColor(String color) {  
        this.color = color;  
    }  
  
    public String getCar() {  
        return car;  
    }  
  
    public void setCar(String car) {  
        this.car = car;  
    }  
  
    //public String getAddTip() {  
    //    return addTip;  
    //}  
    //  
    //public void setAddTip(String addTip) {  
    //    this.addTip = addTip;  
    //}  
  
    @Override  
    public String toString() {  
        return "FlyPig{" +  
                "name='" + name + '\'' +  
                ", color='" + color + '\'' +  
                ", car='" + car + '\'' +  
                ", AGE='" + AGE + '\'' +  
                //", addTip='" + addTip + '\'' +  
                '}';  
    }  
}  
```

注意下，注释的代码，是一会儿要各种情况下使用的。下面就是main方法啦

```java
package com.lxk.test;  
  
import com.lxk.model.FlyPig;  
  
import java.io.*;  
  
/** 
 * 序列化测试 
 * 
 * @author lxk on 2017/11/1 
 */  
public class SerializableTest {  
    public static void main(String[] args) throws Exception {  
        serializeFlyPig();  
        FlyPig flyPig = deserializeFlyPig();  
        System.out.println(flyPig.toString());  
  
    }  
  
    /** 
     * 序列化 
     */  
    private static void serializeFlyPig() throws IOException {  
        FlyPig flyPig = new FlyPig();  
        flyPig.setColor("black");  
        flyPig.setName("naruto");  
        flyPig.setCar("0000");  
        // ObjectOutputStream 对象输出流，将 flyPig 对象存储到E盘的 flyPig.txt 文件中，完成对 flyPig 对象的序列化操作  
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(new File("d:/flyPig.txt")));  
        oos.writeObject(flyPig);  
        System.out.println("FlyPig 对象序列化成功！");  
        oos.close();  
    }  
  
    /** 
     * 反序列化 
     */  
    private static FlyPig deserializeFlyPig() throws Exception {  
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream(new File("d:/flyPig.txt")));  
        FlyPig person = (FlyPig) ois.readObject();  
        System.out.println("FlyPig 对象反序列化成功！");  
        return person;  
    }  
}  
```

对上面的2个操作文件流的类的简单说明 

ObjectOutputStream代表对象输出流： 

它的writeObject\(Object obj\)方法可对参数指定的obj对象进行序列化，把得到的字节序列写到一个目标输出流中。

ObjectInputStream代表对象输入流： 

它的readObject\(\)方法从一个源输入流中读取字节序列，再把它们反序列化为一个对象，并将其返回。

具体怎么看运行情况。

第一种：上来就这些代码，不动，直接run，看效果。

实际运行结果，他会在 d:/flyPig.txt 生成个文件。



![](../../.gitbook/assets/serializable_1.jpeg)

从运行结果上看：

1，他实现了对象的序列化和反序列化。

2，transient 修饰的属性，是不会被序列化的。我设置的奥迪四个圈的车不见啦，成了null。my god。

3，你先别着急说，这个静态变量AGE也被序列化啦。这个得另测。

第二种：为了验证这个静态的属性能不能被序列化和反序列化，可如下操作。

这个完了之后，意思也就是说，你先序列化个对象到文件了。这个对象是带静态变量的static。

现在修改flyPig类里面的AGE的值，给改成26吧。

然后，看下图里面的运行代码和执行结果。



![](../../.gitbook/assets/serializable_2.jpeg)

可以看到，刚刚序列化的269，没有读出来。而是刚刚修改的26，如果可以的话，应该是覆盖这个26，是269才对。

所以，得出结论，这个静态static的属性，他不序列化。

