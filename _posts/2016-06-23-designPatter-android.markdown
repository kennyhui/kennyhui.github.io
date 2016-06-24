---
layout:     post
title:      "android 设计模式"
subtitle:   "android面试"
date:        2016-06-22 09:00:00
author:     ""
header-img: "img/post-bg-android.png"
tags:
   - android interview
---


##面向对象的六大原则


#单一职责原则

所谓职责是指类变化的原因。如果一个类有多于一个的动机被改变，那么这个类就具有多于一个的职责。而单一职责原则就是指一个类或者模块应该有且只有一个改变的原因。通俗的说，即一个类只负责一项职责，将一组相关性很高的函数、数据封装到一个类中。

#开闭原则


　　对于扩展是开放的，这意味着模块的行为是可以扩展的。当应用的需求改变时，我们可以对模块进行扩展，使其具有满足那些改变的新行为。 　　对于修改是关闭的，对模块行为进行扩展时，不必改动模块的源代码。

　　通俗的说，尽量通过扩展的方式实现系统的升级维护和新功能添加，而不是通过修改已有的源代码。

#里氏替换原则


使用“抽象(Abstraction)”和“多态(Polymorphism)”将设计中的静态结构改为动态结构，维持设计的封闭性。任何基类可以出现的地方，子类一定可以出现。

　　在软件中将一个基类对象替换成它的子类对象，程序将不会产生任何错误和异常，反过来则不成立。在程序中尽量使用基类类型来对对象进行定义，而在运行时再确定其子类类型，用子类对象来替换父类对象。

#依赖倒置原则

高层次的模块不应该依赖于低层次的模块，他们都应该依赖于抽象。抽象不应该依赖于具体实现，具体实现应该依赖于抽象。 　　 　　程序要依赖于抽象接口，不要依赖于具体实现。简单的说就是要求对抽象进行编程，不要对实现进行编程，这样就降低了客户与实现模块间的耦合（各个模块之间相互传递的参数声明为抽象类型，而不是声明为具体的实现类）。


#接口隔离原则


一个类对另一个类的依赖应该建立在最小的接口上。其原则是将非常庞大的、臃肿的接口拆分成更小的更具体的接口。

#迪米特原则


又叫作最少知识原则，就是说一个对象应当对其他对象有尽可能少的了解。 　　通俗地讲，一个类应该对自己需要耦合或调用的类知道得最少，不关心被耦合或调用的类的内部实现，只负责调用你提供的方法。

#1.Singleton（单例模式）


作用：　　

    保证在Java应用程序中，一个类Class只有一个实例存在。
好处：

    由于单例模式在内存中只有一个实例，减少了内存开销。
    单例模式可以避免对资源的多重占用，例如一个写文件时，由于只有一个实例存在内存中，避免对同一个资源文件的同时写操作。 　　 单例模式可以再系统设置全局的访问点，优化和共享资源访问。
    

使用情况：

    建立目录 数据库连接的单线程操作
    某个需要被频繁访问的实例对象　


#1.1 使用方法


第一种形式：


public class Singleton {

    /* 持有私有静态实例，防止被引用，此处赋值为null，目的是实现延迟加载 */
    private static Singleton instance = null;
    
    /* 私有构造方法，防止被实例化 */
    private Singleton() {
    }
    
    /* 懒汉式：第一次调用时初始Singleton，以后就不用再生成了
    静态方法，创建实例 */
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
    }

但是这有一个问题，不同步啊！在对据库对象进行的频繁读写操作时，不同步问题就大了。


#第二种形式：


既然不同步那就给getInstance方法加个锁呗！我们知道使用synchronized关键字可以同步方法和同步代码块，所以：　　


     public static synchronized Singleton getInstance() {  
     if (instance == null) {  
         instance = new Singleton();  
     }  
     return instance;  
    } 


或是


     public static Singleton getInstance() {  
     synchronized (Singleton.class) {  
         if (instance == null) {  
             instance = new Singleton();  
         }  
     }  
     return instance;  
    }

获取Singleton实例：

    Singleton.getInstance().方法（）

#1.２android中的Singleton


    软键盘管理的 InputMethodManager   

源码(以下的源码都是5.1的)：

        205 public final class InputMethodManager {
    //.........
    211     static InputMethodManager sInstance;
    //.........
    619     public static InputMethodManager getInstance() {
    620         synchronized (InputMethodManager.class) {
    621             if (sInstance == null) {
    622                 IBinder b = ServiceManager.getService(Context.INPUT_METHOD_SERVICE);
    623                 IInputMethodManager service = IInputMethodManager.Stub.asInterface(b);
    624                 sInstance = new InputMethodManager(service, Looper.getMainLooper());
    625             }
    626             return sInstance;
    627         }
    628     }

使用的是第二种同步代码块的单例模式（可能涉及到多线程），类似的还有 　　AccessibilityManager（View获得点击、焦点、文字改变等事件的分发管理，对整个系统的调试、问题定位等） 　　BluetoothOppManager等。

当然也有同步方法的单例实现，比如：CalendarDatabaseHelper


    307     public static synchronized CalendarDatabaseHelper getInstance(  Context context) {
    308         if (sSingleton == null) {
    309             sSingleton = new CalendarDatabaseHelper(context);
    310         }
    311         return sSingleton;
    312     }

#注意Application并不算是单例模式


    44 public class Application extends ContextWrapper implements ComponentCallbacks2 {
    
    79     public Application() {
    80         super(null);
    81     }

在Application源码中，其构造方法是公有的，意味着可以生出多个Application实例，但为什么Application能实现一个app只存在一个实例呢？请看下面：

在ContextWrapper源码中：

    50 public class ContextWrapper extends Context {
    51     Context mBase;
    52 
    53     public ContextWrapper(Context base) {
    54         mBase = base;
    55     }

    64     protected void attachBaseContext(Context base) {
    65         if (mBase != null) {
    66             throw new IllegalStateException("Base context already set");
    67         }
    68         mBase = base;
    69     }


 ContextWrapper构造函数传入的base为null, 就算有多个Application实例，但是没有通过attach()绑定相关信息，没有上下文环境，三个字。

#２. Factory（工厂模式）


    定义一个用于创建对象的接口，让子类决定实例化哪一个类。工厂方法使一个类的实例化延迟到其子类。 　对同一个接口的实现类进行管理和实例化创建

假设我们有这样一个需求：


动物Animal，它有行为move()。有两个实现类cat和dog。为了统一管理和创建我们设计一个工厂模式。 　　同时两个子类有各自的行为，Cat有eatFish()，Dog有eatBone().


![](https://camo.githubusercontent.com/477bec5efe95c24ab4709c8108728a500c5b3d0f/687474703a2f2f696d672e626c6f672e6373646e2e6e65742f3230313630363230313431303233333933)


Animal接口：


    interface animal {
    void move();
    }


Cat类:

public class Cat implements Animal{

    @Override
    public void move() {
        // TODO Auto-generated method stub
        System.out.println("我是只肥猫，不爱动");
    }
    public void eatFish() {  
        System.out.println("爱吃鱼");
    } 
    }

Dog类:

public class Dog implements Animal{

    @Override
    public void move() {
        // TODO Auto-generated method stub
        System.out.println("我是狗，跑的快");
    }
    public void eatBone() {  
        System.out.println("爱吃骨头");
    } 
    }


那么现在就可以建一个工厂类（Factory.java）来对实例类进行管理和创建了.


    public class Factory {
        //静态工厂方法
        //多处调用，不需要实例工厂类 
         public static Cat produceCat() {  
                return new Cat();  
            } 
         public static Dog produceDog() {  
                return new Dog();  
            }
    //当然也可以一个方法，通过传入参数，switch实现
    }

使用：

    Animal cat = Factory.produceCat();
    cat.move();
    //-----------------------------
    Dog dog = Factory.produceDog();
    dog.move();
    dog.eatBone();


工厂模式在业界运用十分广泛，如果都用new来生成对象，随着项目的扩展，animal还可以生出许多其他儿子来，当然儿子还有儿子，同时也避免不了对以前代码的修改（比如加入后来生出儿子的实例）,怎么管理，想着就是一团糟。

    Animal cat = Factory.produceCat();


这里实例化了Animal但不涉及到Animal的具体子类（减少了它们之间的偶合联系性），达到封装效果，也就减少错误修改的机会。

　　Java面向对象的原则，封装(Encapsulation)和分派(Delegation)告诉我们：具体事情做得越多，越容易范错误，

　　一般来说，这样的普通工厂就可以满足基本需求。但是我们如果要新增一个Animal的实现类panda，那么必然要在工厂类里新增了一个生产panda的方法。就违背了 闭包的设计原则（对扩展要开放对修改要关闭） ，于是有了抽象工厂模式。

#２.1 Abstract Factory(抽象工厂)

抽象工厂模式提供一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类。 　　啥意思？就是把生产抽象成一个接口，每个实例类都对应一个工厂类（普通工厂只有一个工厂类），同时所有工厂类都继承这个生产接口。

生产接口Provider：

    interface Provider {
        Animal produce(); 
    }
每个产品都有自己的工厂 CatFactory：

public class CatFactory implements Provider{

    @Override
    public Animal produce() {
        // TODO Auto-generated method stub
        return new Cat();
    }
    }
DogFactory：

public class DogFactory implements Provider{

    @Override
    public Animal produce() {
        // TODO Auto-generated method stub
        return new Dog();
    }
}
产品生产：

    Provider provider = new CatFactory();
    Animal cat =provider.produce();
    cat.move();
　　
现在我们要加入panda，直接新建一个pandaFactory就行了，这样我们系统就非常灵活，具备了动态扩展功能。

#２.1 Android中的Factory


比如AsyncTask的抽象工厂实现：

工厂的抽象：

    59　public interface ThreadFactory {
    //省略为备注
    69    Thread newThread(Runnable r);
    70 }

产品的抽象（new Runnable就是其实现类）：

    56   public interface Runnable {
    //省略为备注
    68    public abstract void run();
    69 }


