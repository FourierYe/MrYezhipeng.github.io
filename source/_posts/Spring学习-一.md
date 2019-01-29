---
title: Spring学习(一)
date: 2019-01-29 18:52:28
tags:
---
学了快一个月的SSH框架了，写程序遇到Bug无法继续进行，把这段时间学习的知识点整理一下。

### Spring部分
Spring主要分为IOC与AOP两部分，下面我感性的总结一下我学习这两部分的感受。重点放在IOC部分，AOP还没有学完。

### IOC(依赖注入)
IOC(DI) 是靠 Java反射（来实现的）
很多参考书和学习资料上把这部分讲的非常晦涩难懂，其实Spring的IOC机制其实是为了降低模块之间的耦合

高内聚低耦合

举个例子
这是Action层中的一个类
如果不使用IOC的话，是这样实现的：

``` java
package action;

import com.opensymphony.xwork2.ActionSupport;
import com.opensymphony.xwork2.ModelDriven;
import entity.UserManageEntity;
import service.UserManageService;

public class UserManageAction extends ActionSupport implements ModelDriven<UserManageEntity>{
    
    private UserManageEntity userManageEntity=new UserManageEntity();

    public UserManageEntity getModel() {
        return userManageEntity;
    }

    public String userManageJudge(){

        return new UserManageService().userManageJudge(this.userManageEntity); //需要手动new一个UserManageService的对象来使用他的方法。
    }
}
```

如果我们使用Spring的IOC机制的话
我们的代码是这样的：

``` java
package action;

import com.opensymphony.xwork2.ActionSupport;
import com.opensymphony.xwork2.ModelDriven;
import entity.UserManageEntity;
import service.UserManageService;

    public class UserManageAction extends ActionSupport implements ModelDriven<UserManageEntity>{
    private UserManageService userManageService;
    private UserManageEntity userManageEntity=new UserManageEntity();

    public UserManageEntity getModel() {
        return userManageEntity;
    }
    //设置注入UserManageService
    public void setUserManageService(UserManageService userManageService) {
        this.userManageService = userManageService;
    }

    public String userManageJudge(){

        return userManageService.userManageJudge(this.userManageEntity);
    }
}

```

xml文件配置是这样的

``` xml
<bean name="userManageAction" class="action.UserManageAction">
    <property name="userManageService" ref="userManageService"/>
</bean>
```

相当于Spring自动的把UserManageService对象装配到UserManageAction里面，省去了手工new 实例对象的过程，虽然上面代码看起来比手工new繁琐很多，你肯定会想Spring的这种机制是不是过度设计了，手工new的过程方便很多，其实并不是这样。

IOC实现的目的是为了后期的可维护和代码复用性
比如我们这时候想造一辆车，显然我们需要车身，车身又需要底盘，底盘又需要轮子。这种需求关系就叫做依赖

依靠别人或事物而不能自立或自给称为依赖。

我们用代码来表示上述过程。
车类

``` java
public class Car {
    private Framework framework;
    public Car(){
        this.framework=new Framework();
    }
    public void run(){
        System.out.println("车动了");
    }
}
```
车身

``` java
public class Framework {
    private Bottom bottom;
    public Framework(){
        this.bottom=new Bottom();
    }
}
```
底盘

``` java
public class Bottom {
    private Tire tire;
    public Bottom(){
        this.tire=new Tire();
    }
}
```
轮子

``` java
public class Tire {
    private int size;
    public Tire(){
        this.size=30;
    }
}
``` 
显然上述过程是在构造期间注入相关依赖的。
如果我们现在改一下需求把轮子的大小改成动态的，我们是不是需要从下往上依次修改，如下

``` java
public class Tire {
    private int size;
    public Tire(int size){
        this.size=size;
    }
}
public class Bottom {
    private Tire tire;
    public Bottom(int size){
        this.tire=new Tire(size);
    }
}
public class Framework {
    private Bottom bottom;
    public Framework(int size){
        this.bottom=new Bottom(size);
    }
}
......
```

显然如果工程量很大的话，这种设计方式，可维护性和可扩展性非常差。
我们换一种思路，我们把构造器注入依赖的方式改为设值set注入试试看。

``` java
public class Car {
    private Framework framework;
    public Car(){

    }

    public void setFramework(Framework framework) {
        this.framework = framework;
    }

    public void run(){
        System.out.println("车动了");
    }
}

public class Framework {
    private Bottom bottom;
    public Framework(){

    }

    public void setBottom(Bottom bottom) {
        this.bottom = bottom;
    }
}

public class Bottom {
    private Tire tire;
    public Bottom(){

    }

    public void setTire(Tire tire) {
        this.tire = tire;
    }
}

public class Tire {
    private int size;
    public Tire(){

    }

    public void setSize(int size) {
        this.size = size;
    }
}
```
显然这种方式可以随时更改Tire类、Bottom类、Framework类、Car类而不影响其他类。这种set方式就形成了一定程度的依赖解耦，我们把各个功能set的方式交给其他类管理，这就是典型的工厂模式，而我们的Sping就是这种工厂。
其他内容，请见Spring学习感想(二)。
本篇文章参考了部分优秀资料，相关链接如下：
知乎：[Spring IoC的好处]("https://www.zhihu.com/question/23277575")