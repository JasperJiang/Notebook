# Composite 组合模式

组合模式概念：将对象组合成树形结构以表示“部分-整体”的层次结构，使得用户对单个对象和组合对象的使用具有一致性。

例子：网页通常有导航菜单，菜单分为一级菜单、二级菜单、三级菜单甚至更多，此时就适合用组合模式，表示菜单的树形结构。

创建菜单类，同时提供引用自身对象的list

```java
public class Menu {
    /* 菜单等级 */
    String level;
    /* 菜单名称 */
    String name;
    /* 子菜单 */
    List<Menu> menuList = new ArrayList<>();

    public Menu(String level, String name) {
        this.level = level;
        this.name = name;
    }

    public void add(Menu menu) {
        menuList.add(menu);
    }

    public void remove(Menu menu) {
        menuList.remove(menu);
    }

    /** 打印 */
    public void toString(String str) {
        System.out.println(str + "level:" + this.level + ", name:" + this.name);
        str = str + "   ";
        for (Menu menu : menuList) {
            menu.toString(str);
        }
    }
}
```

客户端

```java
Menu rootMenu=new Menu("一级菜单", "首页");

Menu SecMenu1=new Menu("二级菜单","菜单1");

Menu SecMenu2=new Menu("二级菜单","菜单2");

Menu ThrMenu1=new Menu("三级菜单","菜单1");

Menu ThrMenu2=new Menu("三级菜单","菜单2");
//一级菜单放入二级菜单
rootMenu.add(SecMenu1);
rootMenu.add(SecMenu2);
//二级菜单1放入三级菜单
SecMenu1.add(ThrMenu1);
SecMenu1.add(ThrMenu2);

rootMenu.toString("");
```

输出结果

```text
level:一级菜单, name:首页
   level:二级菜单, name:菜单1
      level:三级菜单, name:菜单1
      level:三级菜单, name:菜单2
   level:二级菜单, name:菜单2
```

一个树形的菜单结构就有了。   
组合模式的核心是**定义包含自身对象的集合，从而形成树的结构，让每个节点都有同样的元素和操作，使得用户对单个对象和组合对象的使用具有一致性。**

总结：   
1. 组合模式最终形成树形结构以表示”部分-整体”的层次结构，因此适用于所有需要有层次结构的场景，包括文件结构，树形菜单等。

