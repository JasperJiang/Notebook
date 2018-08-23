# Adapter 适配器模式

适配器模式概念：**把一个类的接口变换成客户端所期待的另一种接口，从而使原本因接口不匹配而无法在一起工作的两个类能够在一起工作。**

例子：我新买的充电器只能充安卓手机的电，但是我朋友的苹果手机也想充电，如果有一个充电器既能充安卓手机、又能充苹果手机的电就好了。此时我们就可以采用适配器模式，在充电器的基础上装一个多功能充电的适配器，不就能够同时充安卓和苹果手机的电了。

先定义我新买的充电器

```java
public class Charger {
    public void chargingForAndroid() {
        System.out.println("我正在充安卓手机的电");
    }
}
```

接着定义我想要安卓手机和苹果手机都能充电的接口

```java
public interface Targetable {
    /* 充安卓手机的电 */
    public void chargingForAndroid();
    /* 充苹果手机的电 */
    public void chargingForIOS();
}
```

然后创建实现该接口的类，也就是所谓的**适配器**，注意这个适配器是在原有充电器的基础上创建的，因此需要继承我的充电器

```java
public class ChargerWithAdapter extends Charger implements Targetable{
    @Override
    public void chargingForIOS() {
        System.out.println("我正在充苹果手机的电");
    }
}
```

OK,我想要的既能充安卓手机，又能充苹果手机电的充电器就有了

```java
Targetable charger = new ChargerWithAdapter();//创建带有适配器的充电器
charger.chargingForAndroid();
charger.chargingForIOS();
```

输出结果

```text
我正在充安卓手机的电
我正在充苹果手机的电
```

其中最核心的地方就是**定义适配器的时候，继承原有的基类，实现满足我们需要的接口。**继承基类能够提供基类已实现的方法，满足我们需要的接口则是在这些方法的基础上再添加一些方法，然后在适配器中完成该实现。

上面是类的适配器模式，也就是基于类然后进行适配，同样也可以基于类中的对象进行适配   
把基类充电器作为适配器的对象来适配，而不是直接继承

```java
public class ChargerWithAdapter implements Targetable {
    // 把基类作为一个对象属性
    private Charger charger;
    // 基类原有的方法则通过对象来完成
    @Override
    public void chargingForAndroid() {
        charger.chargingForAndroid();
    }

    @Override
    public void chargingForIOS() {
        System.out.println("我正在充苹果手机的电");
    }

    public ChargerWithAdapter(Charger charger) {
        this.charger = charger;
    }
}
```

客户端

```java
Charger charger=new Charger();
Targetable target = new ChargerWithAdapter(charger);//创建带有适配器的充电器
target.chargingForAndroid();
target.chargingForIOS();
```

总结：   
1. 适配器模式主要解决**类的复用和兼容整合**问题。   
2. 适配器模式主要适用于**现有的类或对象不能满足新的环境、也就是不能和新的接口匹配的情况，通常是在系统移植或更新优化的时候考虑。**   
3. 由于java只能单继承，而对象的适配器模式可以把需要适配的基类都作为对象放进去，因此对象的适配器模式比类的适配器模式扩展性好很多。

