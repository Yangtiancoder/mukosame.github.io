---
layout: post
title: 创建对象的那些事
category: blog
tags: [Java]
description: 谈谈静态工厂方法。
---

## 静态工厂方法是什么

静态工厂方法描述为一个返回类的实例的静态方法。

举个例子，Boolean的一个将基本类型boolean转为封装类的方法，valueOf：

```java
public static Boolean valueOf(boolean b) {
    return (b ? TRUE : FALSE);
}
```

## 静态工厂方法和设计模式中的工厂模式区别

对于一个类来说，为了让客户端获得一个自身的实例，最常用的方法就是提供一个公有的构造器。除了这种使用构造器的方法之外，对于单个类来说，我们可以定义静态工厂方法来获取自身的类的一个实例。静态工厂方法和工厂模式的不同也体现在此，静态工厂方法是获取这个类自身的一个实例，他的存在是为了更好的描述和处理这个类。而工厂模式的作用更在于解耦，让每个类在实例化的时候不再使用new这种耦合度极高的方法。  
 
我们通过静态工厂方法来代替构造器，我们首先需要知道的是静态工厂方法只是一个“普通的方法”。的确，我们将具有返回这个对象的一个实例这种特点的静态方法叫做静态工厂方法，它在理论上与其他方法并没有什么不同。但是也正是因为他是一种”普通的方法“，它具有很多构造器不具备的优点。 

## 为什么提倡采用静态工厂方法创建对象

**静态工厂方法可以有不同的名字**

对于普通的构造器来说，通过参数来对类中的不同属性赋值，然后返回一个这个类的相应实例。但在有些时候，如果我们想获得有些差别的类实例，唯一可以采用的方法是通过不同的构造器参数列表来区分不同的实例。但是这并不是一个好主意，因为有的时候，仅仅是构造器方法签名上的不同，可能会让客户端用户迷惑，只是参数顺序的变化让他们很难去记住到底哪个构造对应的是哪个实例。

这个时候可以考虑使用静态工厂方法，为不同的构造方法来起不同的名字来区分不同功能的实例化，而返回的都是一个this，这样类似构造器的操作让不同的实例可以被更加容易的区分。

**不必每次调用的时候都创建一个新对象**

每当我们调用构造器，每当我们使用new来初始化一个对象时，都无疑是在堆上创建了一个新的对象。而有些不可变的类、不希望被实例出多个对象的类不用也不必每次都创建一个对象。通过静态工厂方法，我们可以直接向客户端返回一个我们早已创建好的对象，对于有些不可变的类，比如基本类型包装类，这样做可以极大地节省我们的开销。

对于单例模式的类，只允许每个类中存在一个已经实例化的对象。对于单例模式来说，构造器都会被声明为private，我们不能也无法构造一个对象，只能通过静态工厂方法来获取他的对象。这种手法在很多很多包中都有体现，尤其是那些比较重量级的类（其实我们也恰好总喜欢把这种变量其名为factory），更需要这种手法来操作对象。


**可以返回原返回类型的任何子类型的对象**  

设计模式中的基本的原则之一——『里氏替换』原则，就是说子类应该能替换父类。显然，构造方法只能返回确切的自身类型，而静态工厂方法则能够更加灵活，可以根据需要方便地返回任何它的子类型的实例。

```java
Class Person {
    public static Person getInstance(){
        return new Person();
        // 这里可以改为 return new Player() / Cooker()
    }
}
Class Player extends Person{
}
Class Cooker extends Person{
}
```

## 多个构造器参数考虑builder

当一个类的有很多的属性时，为它创建构造器可以使用重叠构造器模式，及每一个构造器都比前一个构造器多一个属性。重叠构造器模式可行，但是当有许多参数的时候，客户端代码会很难编写，并且仍然较难以阅读。遇到许多构造器参数的时候，还有第二种代替办法，即JavaBeans模式，在这种模式下，调用一个无参构造器来创建对象，然后调用setter方法来设置每个必要的参数，以及每个相关的可选参数。
遗憾的是，JavaBeans模式自身有着很严重的缺点。因为构造过程被分到了几个调用中，在构造过程中JavaBean可能处于不一致的状态。类无法仅仅通过检验构造器参数的有效性来保证一致性。试图使用处于不一致状态的对象，将会导致失败，这种失败与包含错误的代码大相径庭，因此他调试起来十分困难。而且，JavaBeans模式阻止了把类做成不可变的可能。这就需要程序员付出额外的努力来确保它的线程安全。

未完。。。
