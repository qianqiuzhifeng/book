# Effective Java

## 创建和销毁对象

### 第1条 考虑用静态工厂方法替代构造器

静态工厂方法与构造器相比的优势有：

* 静态工厂方法有名称

* 不必在每次调用它们的时候都创建一个新的对象

* 它们可以返回原返回类型的任何子类型的对象

* 在创建参数化实例的时候，使得代码变得更加简洁

静态工厂方法的主要缺点：

* 类如果不含公有的或受保护的构造器，就不能被子类化

* 它们与其他静态方法实际上没有任何区别

### 第2条 遇到多个构造器参数时要考虑用构建器

构建器参见《Java设计模式》第二版

### 第3条 用私有构造器或者枚举类型强化Singleton属性

### 第4条 通过私有构造器强化不可实例化的能力

### 第5条 避免创建不必要的对象

### 第6条 消除过期的对象引用

目的是避免内存泄露

内存泄露常见来源：

* 过期引用

* 缓存

* 监听器和其他回调

### 第7条 避免使用终结方法

## 对所有对象都通用的方法

### 第8条 覆盖equals方法时请遵守通用约定

约定内容：

equals方法实现了等价关系。

* 自反性。对于任何非null的引用值x, x.equals(x)必须返回True。

* 对称性。对于任何非null的引用值x和y，当且仅当y.equals(x)返回true时，x.equals(y)必须返回为true。

* 传递性。对于任何非null的引用值x,y和z。如果x.equals(y)返回true，并且y.equals(z)也返回true，那么x.equals(z)也必须返回true。

* 一致性。对于任何非null的引用值x和y，只有equals的比较操作对象中所用的信息没有被修改，多次调用x.equals(y)就会一致地返回true，或者一致地返回false。

对于任何非null的引用值x，x.equals(null)必须返回false。

### 第9条 覆盖equals时总是要覆盖hashCode

### 第10条 始终要覆盖toString

### 第11条 谨慎地覆盖clone

### 第12条 考虑实现Comparable接口

## 类和接口

### 第13条 使类和成员的可访问性最小化

访问级别：

* 私有的(private)

```text
只有在声明该成员的顶层类内部才可以访问这个成员
```

* 包级私有的(package-private)

```text
声明该成员的包内部任何类都能够访问这个成员。从技术上讲，它被称为“缺省(default)访问级别”，如果没有为成员指定访问修饰符，就采用这个访问级别。
```

* 受保护的(protected)

```text
声明该成员类的子类可以访问这个成员(但有一些限制)，并且声明该成员的包的任何类也可以访问该成员。
```

* 公有的(public)

```text
在任何地方都可以访问该成员。
```

### 第14条 在公有类中使用访问方法而非公有域

### 第15条 使可变性最小化

不可变类比可变类更加易于设计、实现和使用，他们不容易出错，且更加安全。

实现不可变类，遵循以下原则：

* 不要提供任何修改类状态的方法。

* 保证类不会被扩展。

* 使所有的域都是final的。

* 使所有的域都成为私有的。

* 确保对任何可变组件的互斥访问。

### 第16条 复合优先与继承

### 第17条 要么为继承而设计，并提供文档说明，要么就禁止继承

### 第18条 接口优于抽象类

### 第19条 接口只用于定义类型

### 第20条 类层次优于标签类

### 第21条 用函数对象表示策略

### 第22条 优先考虑静态成员类

嵌套类：

* 静态成员类

(1)静态成员类可以看做普通的类。
(2)静态成员类可以访问外围类的所有成员，包括那些声明为私有的成员。
(3)静态成员类是外围内的静态成员，与其他静态成员一样，遵守同样的可访问性规则。

静态成员类的一种常见用法是作为公用的辅助类，仅当它与外部类一起使用时才有意义。

私有静态成员类的一种常见用法是作为外围类所代表的对象的组件。

```java
public class Outer {

    private static final String PARAM = "Param";
    //静态成员类
    public static final class StaticClass{
        private StaticClass(){
        }
        private static final void print(){
            System.out.print(PARAM);
        }
    }
    public static void main(String[] args){
        StaticClass.print();
    }
}
````

* 非静态成员类

静态成员类与非静态成员类的区别：

(1)语法上：非静态成员类没有static修饰符。
(2)用法上：非静态成员类的每个实例都隐含着与一个外围类的一个外围实例相关联。

非静态成员类的一种常见的用法是定义一个Adapter,它允许外部类的实例被看作是另一个不相关类的实例。

```java
package qian.qiu.zhi.feng;

public class Outer {

    private static final String PARAM = "Param";

    public class  UnStaticClass{
        private void print(){
            System.out.print(PARAM);
        }
    }
    public static void main(String[] args){
        new Outer().new UnStaticClass().print();
    }
}

```

* 匿名类

(1)匿名类的声明是由java编译器自动派生自一个类实例创建表达式。
(2)匿名类永远不能是抽象的。
(3)匿名类总是隐式的final。
(4)匿名类总是一个内部类；并且不能是static的。

```java

interface D{
    void ShowContext();
}

new D(){
    @Override
    public void ShowContext() {
        System.out.println("hello");
    }
}
```

* 局部类

(1)在任何”可以声明局部变量“的地方，都可以声明局部类，并且局部类也遵守同样的作用域规则。

```java
public class Test {
    {
        class AA{}//块内局部类
    }
    public Test(){
        class AA{}//构造器内局部类
    }
    public static void main(String[] args){
    }
    public void test(){
        class AA{}//方法内局部类
    }
}
```

## 泛型

### 第23条 请不要在代码中使用原生态类型

