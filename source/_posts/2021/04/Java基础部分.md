---
title: Java基础部分
date: 2021-04-22 14:29:32
tags:
 - 面试
 - Java
categories:
 - 面试
math:
---

### JAVA基础

#### 面向对象的特性

1. 封装：将事物封装成一个类，**减少耦合，隐藏细节**。保留特定的接口与外界联系，当接口内部发生改变时，不会影响外部调用。
   比如将类的属性私有化，只有**通过公共的get/set方法才能进行访问**，在get/set方法中我们可以对**内部逻辑进行封装处理**，外部的调用方法不必关心我们的处理逻辑。

2. 继承：从一个已知的类中**派生出一个新的类**（子类），新的类可以使用已知类（即父类）的行为和属性，并且可以**通过覆盖/重写**来增强已知类的能力。
   JAVA不支持多继承，并且JAVA的构造函数不可以被继承。
   构造函数被private修饰的话，其就是不明确的构造函数，该类不可以被其他类继承。
   JAVA中类的初始化顺序：

   1. 初始化**父类**中的**静态成员变量和静态代码块**
   2. 初始化**子类**中的**静态成员变量和静态代码块**
   3. 初始化**父类**中的普通成员变量和代码块，再**执行父类的构造方法**
   4. 初始化**子类**中的普通成员变量和代码块，再**执行子类的构造方法**

   子类的特点：

   1. 子类**拥有**父类非private的属性和方法
   2. 子类可以添加自己的方法和属性，即**对父类进行扩展**
   3. 子类可以重新定义父类的方法，即**方法的覆盖/重写**

3. 多态：多态的本质是一个程序中存在多个同名的不同方法，主要通过三种方式来实现

   1. 通过子类对父类的**覆盖（重写）**来实现
   2. 通过在一个类中对方法的**重载**来实现
   3. 通过将**子类对象作为父类对象**使用来实现

<!-- more -->

##### 覆盖（@Override）

**覆盖也叫重写**，是指**子类和父类**的方法之间的一种关系，比如父类拥有方法A，子类扩展了方法A并添加了丰富的功能。那么我们就说子类覆盖或重写了方法A，也就是说子类中的方法与父类中继承的方法有完全相同的返回值类型、方法名、参数个数及参数类型。

##### 重载

**重载是指在一个类中（包括父类）存在多个同名的不同方法**，这些方法的**参数个数、顺序及类型不同**均可以构成方法的重载。如果仅仅是修饰符、返回值、抛出异常不同，那么是两个相同的方法。

如果只有返回值不同，可以构成重载吗？

不可以。因为我们调用某个方法的时候，不一定关心返回值，此时编译器无法根据方法名和参数确定我们调用的是哪个方法。

##### 向上转型

子类对象转为父类，父类可以是接口。

公式：Father f = new Son();Father是父类或接口，Son是子类。

##### 向下转型

父类对象转为子类。公式：Son s = (Son) f；

向上转型时可以直接转，但是向下转型时必须进行强制类型转换。**该父类对象必须实际指向了一个子类对象才可强制类型向下转型。**

#### JDK，JRE和JVM的区别和联系有哪些？

* **JDK（Java Development Kit）**是一个开发工具包，是Java开发环境的核心组件，并且提供编译、调试和运行一个Java程序所需要的所有工具、可执行文件和二进制文件，是一个平台特定的软件。
* **JRE（Java Runtime Environment）**是指Java运行时环境，是JVM的实现，提供了运行Java程序的平台。JRE包含了JVM，但是不包含Java编译器/调试器之类的开发工具
* **JVM（Java Virtual Machine）**是指Java虚拟机，当我们运行一个程序时，JVM负责将字节码转换为特定的机器代码，JVM提供了内存管理/垃圾回收和安全机制等

区别和联系：

* JDK是开发工具包，用来开发Java程序，而JRE是Java的运行时环境
* JDK和JRE中都包含了JVM
* JVM是Java编程的核心，独立于硬件和操作系统，具有平台无关性，而这也是Java程序可以一次编写，多处执行的原因

##### Java语言的平台无关性是如何实现的？

* JVM屏蔽了操作系统和底层硬件的差异
* Java面向JVM编程，先编译生成字节码文件，然后交给JVM解释成机器码执行
* 通过规定基本数据类型的取值范围和行为，在各个平台上保持一致

##### Java语言是编译型还是解释型语言？

Java的执行经历了编译和解释的过程，是一种**先编译、后解释**执行的语言，不可以单纯归到解释性或编译性语言的类别中。

#### 抽象类和接口有什么区别？

- 抽象类中可以没有抽象方法，也可以抽象方法和非抽象方法共存
- 接口中的方法在JDK8之前只能是抽象的，从JDK8版本开始提供了接口中方法的default实现
- 抽象类和类一样是单继承的；接口可以实现多个父接口
- 抽象类中可以存在普通的成员变量；接口中的变量必须是static final类型的，必须被初始化，接口中只有常量，没有变量
- 在Java中，我们通过abstract来定义抽象类，通过interface关键字来定义接口。

##### 抽象类和接口应该如何选择？分别在什么情况下使用？

根据抽象类和接口的不同之处，当我们仅仅需要定义一些抽象方法而不需要其余额外的具体方法或者变量的时候，我们可以使用接口。反之，则需要使用抽象类，因为抽象类中可以有非抽象方法和变量。

##### JDK8中为什么会出现默认方法呢？

使用接口，使得我们可以面向抽象编程，但是其中一个缺点就是当接口中有改动的时候，需要修改所有的实现类（因为实现接口必须实现其中的抽象方法）。在JDK8中，为了给已经存在的接口增加新的方法并且**不影响现有的实现**，所以引入了接口中的默认方法实现。

默认方法允许在不打破现有继承体系的基础上改进接口，解决接口的修改与现有的实现不兼容的问题。在实际开发中，接口的默认方法应该谨慎使用，因为在复杂的继承体系中，默认方法可能引起歧义和编译错误。

#### Object类的方法

（1）clone方法

保护方法，实现对象的浅复制，只有实现了Cloneable接口才可以调用该方法，否则抛出CloneNotSupportedException异常。

（2）getClass方法

final方法，获得运行时类型。

（3）toString方法

该方法用得比较多，一般子类都有覆盖。

（4）finalize方法

该方法用于释放资源。因为无法确定该方法什么时候被调用，很少使用。

（5）equals方法

该方法是非常重要的一个方法。一般equals和==是不一样的，但是在Object中两者是一样的。子类一般都要重写这个方法。

（6）hashCode方法

该方法用于哈希查找，重写了equals方法一般都要重写hashCode方法。这个方法在一些具有哈希功能的Collection中用到。

一般必须满足``obj1.equals(obj2)==true。可以推出obj1.hash- Code()==obj2.hashCode()``，但是hashCode相等不一定就满足equals。不过为了提高效率，应该尽量使上面两个条件接近等价。

（7）wait方法

wait方法就是使当前线程等待该对象的锁，当前线程必须是该对象的拥有者，也就是具有该对象的锁。wait()方法一直等待，直到获得锁或者被中断。wait(long timeout)设定一个超时间隔，如果在规定时间内没有获得锁就返回。

调用该方法后当前线程进入睡眠状态，直到以下事件发生。

（1）其他线程调用了该对象的notify方法。

（2）其他线程调用了该对象的notifyAll方法。

（3）其他线程调用了interrupt中断该线程。

（4）时间间隔到了。

此时该线程就可以被调度了，如果是被中断的话就抛出一个InterruptedException异常。

（8）notify方法

该方法唤醒在该对象上等待的某个线程。

（9）notifyAll方法

该方法唤醒在该对象上等待的所有线程。

#### Java中的八种基本数据类型及其取值范围

Java的8种基本数据类型分别是：**byte，short，int，long，float，double，char及boolean**。boolean类型的取值为true和false两种，其余每一种基本类型都占有一定的字节，并且拥有着最大值和最小值。比如int的取值范围为Integer.MIN_VALUE到Integer.MAX_VALUE。

每种基本类型所占用的字节数：

* byte：1字节
* short：2字节
* int：4字节
* long：8字节
* float：4字节
* double：8字节
* char：2字节
* boolean：Java规范中没有规定boolean类型所占字节数

各个类型的取值范围：

![image-20210420160235367](https://raw.githubusercontent.com/juhick/picJuhick/master/20210420160235.png)

#### Java中的元注解有哪些？

Java提供了4个元注解，元注解的作用是负责注解其他注解。

* **@Target：**

说明注解所修饰的对象范围，关键源码如下：

```java
public @interface Target {  
    ElementType[] value();  
}  
public enum ElementType {  
  TYPE,FIELD,METHOD,PARAMETED,CONSTRUCTOR,LOCAL_VARIABLE,ANNOCATION_TYPE,PACKAGE,TYPE_PARAMETER,TYPE_USE  
}  

```

例如，如下的注解使用@Target标注，表明MyAnn注解只能作用在类/接口和方法上。

```java
@Target({ElementType.TYPE, ElementType.METHOD})  
public @interface MyAnn {  
}
```

* **@Retention：（保留策略）**

保留策略定义了该注解被保留的时间长短。关键源码如下：

```java
public @interface Retention {  
    RetentionPolicy value();  
}  
public enum RetentionPolicy {  
    SOURCE, CLASS, RUNTIME  
} 
```

其中，**SOURCE：**表示在源文件中有效（即源文件保留）；CLASS：表示在class文件中有效（即class保留）；**RUNTIME：**表示在运行时有效（即运行时保留）。例如，**@Retention(RetentionPolicy.RUNTIME)**标注表示该注解在运行时有效。

* **@Documented：**

该注解用于描述其他类型的注解应该被作为被标注的程序成员的公共API，因此可以被javadoc此类的工具文档化。Documented是一个标记注解，没有成员。关键源码如下：

```java
public @interface Documented {
}
```

* **@Inherited：**

该注解是一个标记注解，@Inherited阐述了某个被标记的类型是被继承的。如果一个使用了@Inherited修饰的注解类型被用于一个class，则这个annotation将被用于该class的子类。关键源码如下：

```java
public @interface Inherited {
}
```

##### 注解的作用

代替繁杂的配置文件，简化开发

##### 如何定义一个注解？

定义注解类不能使用class、enum以及interface，必须使用@interface。下边是一个简单的注解定义：

```java
public @interface MyAnn{}  
```

##### 如何定义注解的属性？

```java
public @interface MyAnn {  
    String value();  
    int value1();  
}
// 使用注解MyAnn，可以设置属性
@MyAnn(value1=100,value="hello")  
public class MyClass {  
} 
```

定义注解时候的value就是属性，看着是一个方法，当我们称其为属性。当为注解指定属性后，那么在使用注解时就必须要给属性赋值了。

#### 说说Java中的反射机制？

**反射机制**是指在运行中，对于任意一个类，都能够知道这个类的所有属性和方法。对于任意一个对象，都能够调用它的任意一个方法和属性。即**动态获取信息和动态调用对象方法的功能**称为反射机制。

##### 反射机制的作用：

* 在运行时判断任意一个对象所属的类
* 在运行时构造一个类的对象
* 在运行时判断任意一个类所具有的成员变量和方法
* 在运行时调用任意一个对象的方法，生成动态代理

##### 与反射相关的类：

* **Class：表示类**，用于获取类的相关信息
* **Field：表示成员变量**，用于获取实例变量和静态变量等
* **Method：表示方法**，用于获取类中的方法参数和方法类型等
* **Constructor：表示构造器**，用于获取构造器的相关参数和类型等

##### 例如，获取Class类有三种基本方式：

1. 通过类名称.class来获取Class类对象：

   ```java
   Class c = int.class；
   Class c = int[ ].class；
   Class c = String.class
   ```

2. 通过对象.getClass()方法来获取Class类对象：

   ```java
   Class c = obj.getClass( );
   ```

3. 通过类名称加载类Class.forName()，只要有类名称就可以得到Class：

   ```java
   Class c = Class.forName("cn.ywq.Demo");
   ```

接下来，是一个通过反射方式来创建对象的Demo：

```java
package com.qhq;

public class Demo1 {
    public static void main(String[] args) throws Exception {
        String className = "com.qhq.User";
        // 获取Class对象
        Class clazz = Class.forName(className);
        // 创建User对象
        User user = (User)clazz.newInstance();
        // 和普通对象一样，可以设置属性值
        user.setUsername("test");
        user.setPassword("123");

        System.out.println(user);
    }
}

class User {
    private String username;
    private String password;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    @Override
    public String toString() {
        return "User [username=" + username + ", password=" + password + "]";
    }
}

```

#### Java中的Exception和Error有什么区别？

* **Exception**是程序正常运行中**预料到可能会出现**的错误，并且应该被捕获并进行相应的处理，是一种**异常**现象
* **Error**是正常情况下不应该出现的，**Error会导致JVM处于一种不可恢复的状态**，不需要进行捕获处理

##### 异常的类型

Exception又分为**运行时异常**和**编译时异常**。

**编译时异常（受检异常）**表示当前调用的方法体内部抛出了一个异常，所以编译器检测到这段代码在运行时可能会出异常，所以要求我们必须对异常进行相应的处理，可以捕获异常或抛给上层调用方。

**运行时异常（非受检异常）**表示在运行时出现的异常，常见的运行时异常包括：空指针异常，数组越界异常，数字转换异常以及算数异常等。

异常应该被捕获，我们可以使用try-catch-finally来处理异常，并且使得程序恢复正常。

##### 捕获异常应该遵循哪些规则呢？

* 尽可能捕捉比较详细的异常，并对每个异常进行相应的处理，而不是使用Exception一起捕获。
* 当本模块不知道捕获之后该怎么处理异常时，可以将其抛给上层模块，上层模块拥有更多的业务逻辑，可以进行更好地处理。
* 捕获异常后至少应该又日志记录，方便之后的排查。
* 不要使用一个很大的try-catch包住整段代码，不利于问题的排查。

##### NoClassDefFoundError和ClassNotFoundException有什么区别？

前者是一个错误，如果JVM或ClassLoader实例尝试加载（可以通过正常的方法调用，也可能是使用new来创建新的对象）类的时候缺找不到类的定义。但是要**查找的类在编译时是存在的，运行的时候却找不到了**。出现这种情况，一**般是由于打包的时候漏掉了部分类或Jar包被篡改已经损坏。**

后者是一个异常，当我们使用例如Class.forName方法来动态加载该类的时候，传入了一个类名，但是其并没有在类路径中被找到的时候，就会导致该异常。**出现这种情况，一般都是类名字传入有误导致的。**

#### JIT编译器有了解么？

Java是一种先编译，后解释执行的语言。

JIT编译器全名为**Just in Time Compile**也就即时编译器，把经常运行的代码**作为“热点代码”编译成与本地平台相关的机器码**，并进行各种层次的优化。JIT编译器除了具有缓存的功能外，还会对代码做各种优化，包括**逃逸分析**、锁消除、锁膨胀、方法内联、空值检查消除、类型检查消除以及公共子表达式消除等。

##### 逃逸分析

逃逸分析的基本行为就是分析对象动态作用域，当一个对象在方法中被定义后，它可能被外部方法所引用，例如作为调用参数传递到其他地方，称为**方法逃逸**。JIT编译器的优化包括如下：

* **同步省略**：也就是锁消除，当JIT编译器判断不会产生并发问题，那么会将同步锁synchronized去掉。
* **标量替换**

标量和聚合量：

* **标量（Scalar）**是指一个无法再分解成更小的数据的数据。Java中的原始数据类型就是标量。
* **聚合量（Aggregate）**是还可以分解的数据。Java中的对象就是聚合量，因为他可以分解成其他聚合量和标量。

在JIT阶段，如果经过逃逸分析，发现一个对象不会被外界访问的话，那么经过JIT优化，就会把这个对象拆解成若干个其中包含的若干个成员变量来代替。这个过程就是标量替换。标量替换的好处就是对象可以不在堆内存进行分配，为栈上分配提供了良好的基础。

##### 逃逸分析的缺点？

目前技术尚不成熟，分析过程耗时长，如果没有一个对象是不逃逸的，那么就得不偿失了。

#### Java中的值传递和引用传递

* **值传递**，意味着传递了对象的一个副本，即使副本被改变，也不会影响源对象。
* **引用传递**，意味着传递的并不是实际的对象，而是对象的引用。因此外部对引用对象的改变会反映到所有对象上。

##### 样例

值传递的例子：

```java
public class Test {
    public static void main(String[] args)  {
        int x=0;
        change(x);
        System.out.println(x);
    }
    static void change(int i){
        i=7;
    }
}
```

毫无疑问，上边的代码会输出0。因为如果参数是基本数据类型，那么是属于值传递的范畴，传递的其实是源对象的一个copy副本，不会影响源对象的值。

下面是一个引用传递的例子：

```java
public class Test {
    public static void main(String[] args)  {
        StringBuffer x = new StringBuffer("Hello");
        change(x);
        System.out.println(x);
    }
    static void change(StringBuffer i) {
        i.append(" world!");
    }
}
```

运行程序后会发现，**输出为Hello world！**下面是执行过程中的内存变化。

![图片说明](https://raw.githubusercontent.com/juhick/picJuhick/master/20210420194126.png)

从图中可以看出**x和i指向了同样的内存地址**，那么i.append操作将直接修改内存地址里边的值，所以当方法结束，**局部变量i消失**，先前变量x所指向的内存值发生了变化，**所以输出为Hello world！**

接下来，将change方法修改为如下方式：

```java
public class Test {
    public static void main(String[] args)  {
        StringBuffer x = new StringBuffer("Hello");
        change2(x);
        System.out.println(x);
    }
    static void change2(StringBuffer i) {
        i = new StringBuffer("hi");
        i.append(" world!");
    }
}
```

上边的代码将会输出Hello，画图分析其内存变化：

![图片说明](https://raw.githubusercontent.com/juhick/picJuhick/master/20210420194443.png)

由图中我们可以看出来，**在函数change2中将引用变量i重新指向了堆内存中另一块区域**，下边都是对另一块区域进行修改，**所以输出是Hello**。

最后，将代码修改如下：

```java
public class Test {
    public static void main(String[] args)  {
        StringBuffer sb = new StringBuffer("Hello ");
        System.out.println("Before change, sb = " + sb);
        changeData(sb);
        System.out.println("After change, sb = " + sb);
    }
    public static void changeData(StringBuffer strBuf) {
        StringBuffer sb2 = new StringBuffer("Hi，I am ");
        strBuf = sb2;
        sb2.append("World!");
    }
}
```

两次输出是相同的

#### String、StringBuffer和StringBuilder的区别

**String：**字符串常量

**StringBuffer：**字符串变量（线程安全）

**StringBuilder：**字符串变量（线程不安全）

String类是用final修饰的，引用内存中的值不可变。引用数据量一大就不效率。StringBuffer和StringBuilder都是用于频繁修改的字符串的，前者是线程安全的，后者是线程不安全的。

如果少量的字符串操作采用String，如果单线程下操作大量字符串采用StringBuilder ，如果多线程下操作大量字符串采用StringBuffer 。StringBuilder的性能比StringBuffer的效率高。

String的+号拼接操作是由StringBuilder实现的，但是直接使用String进行大量修改的话，每次都会指向新的对象，也就是会产生垃圾对象，垃圾对象多了会使JVM启动垃圾回收机制，消耗大量时间。

#### Java中泛型的理解

**泛型的本质使参数化类型**，泛型就是将所操作的数据类型作为参数的一种语法。

```java
public class Play<T>{
    T play(){}
}
```

其中`T`就是作为 类型参数在Play被实例化的时候所传递来的参数，比如：

```java
Play<Integeter> playInteger = new Play<>();
```

这里`T`就会被实例化`Integer`

##### 泛型的实现

泛型的实现过程叫作**擦除**，泛型是为了将具体的类型作为参数传递给方法、类、接口，擦除是在代码运行过程中将具体的类型都擦除。泛型会在编译的时候做类型检查，编译成字节码时会自动进行强制类型转换，减少了Java 1.5之前手动强转可能存在的错误。

##### 泛型的作用

* 使用泛型能写出更加灵活通用的代码
* 泛型将代码安全性检查提前到编译期
* 泛型能够省去类型强制转换
  Java 1.5之前，Java容器都是通过将类型向上转型为`Object`类型来实现的，因此在从容器中取出来的时候需要手动的强制转换。
  加入泛型后，由于编译器知道了具体的类型，因此编译期会自动进行强制转换，使得代码更加优雅。

#### Java序列化与反序列化的过程

**Java序列化**是指把Java对象转换为字节序列的过程；而**Java反序列化**是指把字节序列恢复为Java对象的过程。

##### 进行序列化和反序列化的好处

1. 实现了数据的持久化，通过序列化可以把数据永久地保存到硬盘中（通常存放在文件里）
2. 采用序列化实现远程通信，即在网络上传送对象的字节序列

##### 如何实现Java序列化与反序列化

1. JDK类库中序列化API
   **java.io.ObjectOutputStream：表示对象输出流**

   它的writeObject(Object obj)方法可以对参数指定的obj对象进行序列化，把得到的字节序列写到一个目标输出流中。

   **java.io.ObjectInputStream：表示对象输入流**

   它的readObject()方法从源输入流中读取字节序列，再把它们反序列化成为一个对象，并将其返回。

2. 实现序列化的要求
   只有实现了Serializable或Externalizable接口的类的对象才能被序列化，否则抛出异常。

3. 实现Java对象序列化与反序列化的方法

   假定一个Student类，它的对象需要序列化，可以有如下三种方法：

   1. 若Student类仅仅实现了Serializable接口，则可以按照以下方式进行序列化和反序列化

      ObjectOutputStream采用默认的序列化方式，对Student对象的非transient的实例变量进行序列化。

      ObjcetInputStream采用默认的反序列化方式，对对Student对象的非transient的实例变量进行反序列化。

   2. 若Student类仅仅实现了Serializable接口，并且还定义了readObject(ObjectInputStream in)和writeObject(ObjectOutputSteam out)，则采用以下方式进行序列化与反序列化。

      ObjectOutputStream调用Student对象的writeObject(ObjectOutputStream out)的方法进行序列化。

      ObjectInputStream会调用Student对象的readObject(ObjectInputStream in)的方法进行反序列化。

   3. 若Student类实现了Externalnalizable接口，且Student类必须实现readExternal(ObjectInput in)和writeExternal(ObjectOutput out)方法，则按照以下方式进行序列化与反序列化。

      ObjectOutputStream调用Student对象的writeExternal(ObjectOutput out))的方法进行序列化。

      ObjectInputStream会调用Student对象的readExternal(ObjectInput in)的方法进行反序列化。

#### equals和hashCode方法的关系

二者同是Object类中的对象

```java
public boolean equals(Object obj)
public int hashCode()
```

* equals()：用来判断两个对象是否相同，在Object类中是通过判断对象间的内存地址来决定是否相同
* hashCode()：获取哈希码，也成为散列码，返回一个int整数，这个哈希码的作用是确定该对象在哈希表中的索引位置。

##### 两者的关系

* hashCode主要用于提升查询效率从而提高哈希表性能，来确定在散列结构中对象的存储地址
* 重写equals()必须重写hashCode()
* 哈希存储结构中，添加元素重复性校验的标准就是先检查hashCode值，后判断equals()
* 两个对象equals()相等，hashCode()必定相等
* 两个对象hashCode()不等，equals()必定也不等
* 两个对象hashCode()相等，对象不一定相等，需要通过equals()进一步判断

#### Java和C++的区别

Java和C++都是面向对象语言。

C++为了兼容C多多少少影响了其面向对象的彻底性，而Java是完全的面向对象语言。

1. 指针
   Java中编写程序不能使用指针，减少了指针操作失误所带来的问题，Java虚拟机内部还是使用了指针
2. 多重继承
   C++支持多重继承，允许许多父类派生一个类。尽管多重继承功能很强，但使用复杂，而且会引起许多麻烦，编译程序实现它也很不容易。Java不支持多重继承，但允许一个类继承多个接口（extends+implement），实现了C++多重继承的功能，又避免了C++中的多重继承实现方式带来的诸多不便。
3. 数据类型及类
   Java是完全面向对象的语言，所有函数和变量必须是类的一部分。除了基本数据类型之外，其余的都作为类对象，包括数组。C++允许将函数和变量定义为全局的。此外，Java中取消了C/C++中的结构和联合，消除了不必要的麻烦。
4. 自动内存管理
   Java程序中所有的对象都是用new操作符建立在内存堆栈上，这个操作符类似于C++的new操作符。Java自动进行无用内存回收操作，不需要程序员进行删除。而C++中必须由程序员释放内存资源，增加了程序设计者的负担。Java中当一个对象不被再用到时，无用内存回收器将给它加上标签以示删除。JAVA里无用内存回收程序是以线程方式在后台运行的，利用空闲时间工作。
5. 操作符重载
   Java不支持操作符重载，是为了保持Java语言尽可能简单。
6. 预处理功能
   Java不支持预处理功能。C/C++在编译过程中都有一个预编译阶段，即预处理器。预处理器为开发人员提供了方便，但增加了编译的复杂性。Java虚拟机没有预处理器，但它提供的引入语句(import)与C++预处理器的功能类似。
7. Java不支持缺省函数参数，而C++支持
8. Java不提供goto语句
9. C和C++中有时出现数据类型的隐含转换，Java不支持自动强制类型转换，必须由程序显示进行强制类型转换。

#### 静态与非静态的区别

静态所修饰的在类中是唯一的，static可以修饰成员变量、方法、代码块。静态成员变量和静态方法属于类本身，在类装载的时候被装载到内存中，不自动进行销毁，会一直存在内存中，直到JVM关闭。

首先，两者本质上的区别是：静态方法是在类中使用static修饰的方法，在类定义的时候已经被装载和分配。而非静态方法是不加static关键字的方法，在类定义时没有占用内存，只有在类被实例化成对象时，对象调用该方法才被分配内存。非静态变量和方法属于实例对象，实例化之后才会分配内存，必须通过类的实例来引用，当实例对象被JVM回收之后，也跟着消失。

其次，静态方法中只能调用静态成员或者静态方法，不能调用非静态方法或者非静态成员，而非静态方法既可以调用静态成员或者方法又可以调用其他的非静态成员或者方法。

#### Java中equals方法和==的区别

==：

==是直接比较的两个对象的堆内存地址，如果相等，则说明这两个引用实际是指向同一个对象地址的。

对于基本数据类型（byte，short，char，int，float，double，long，boolean）来说，他们是作为常量在方法区中的常量池里面以HashSet策略存储起来的，对于这样的字符串 "123" 也是相同的道理，在常量池中，一个常量只会对应一个地址，因此不管是再多的 123,"123" 这样的数据都只会存储一个地址，所以所有他们的引用都是指向的同一块地址，因此基本数据类型和String常量是可以直接通过==来直接比较的。

另外，对于基本数据的包装类型（Byte, Short, Character，Integer，Float, Double，Long,  Boolean）除了Float和Double之外，其他的六种都是实现了常量池的，因此对于这些数据类型而言，一般我们也可以直接通过==来判断是否相等。

Integer 在常量池中的存储范围为[-128,127]，127在这范围内，因此是直接存储于常量池的，而128不在这范围内，所以会在堆内存中创建一个新的对象来保存这个值，所以m，n分别指向了两个不同的对象地址，故而导致了不相等。

#### 三大集合接口的引出

Java中的集合，从上层接口看分为了两类，**Map和Collection**。Collection接口的子接口又包括了Set和List接口。这样**常见的Map，Set和List三大集合**接口就出来了。接口类图如下所示：

![图片说明](https://raw.githubusercontent.com/juhick/picJuhick/master/20210420230156.png)

Map是和Collection并列的集合上层接口，没有继承关系；List和Set是Collection的子接口。

#### Java中常见的集合

* **Map接口和Collection接口是所有集合框架的父接口**
* Collection接口的子接口包括：Set接口和List接口
* Map接口的实现类主要有：**HashMap**、TreeMap、Hashtable、LinkedHashMap、**ConcurrentHashMap**以及Properties等
* Set接口的实现类主要有：HashSet、TreeSet、LinkedHashSet等
* List接口的实现类主要有：**ArrayList**、**LinkedList**、Stack以及Vector等

#### HashMap和Hashtable的区别有哪些？

* HashMap没有考虑同步，是线程不安全的；Hashtable使用了synchronized关键字，是线程安全的；
* HashMap允许null作为Key，Hashtable不允许null作为Key，Hashtable的Value也不能为null

##### HashMap线程不安全的例子

* HashMap线程不安全主要是在**多线程环境下进行扩容**可能会出现**HashMap死循环**
* HashMap的线程不安全问题还体现在数据丢失上，也就是多个线程在进行put操作的时候，存在数据的覆盖问题
* Hashtable线程安全是由于其内部实现在put和remove等方法上使用了synchronized进行了同步，所以对**单个方法的使用是线程安全**的。但是对多个方法进行**复合操作时，线程安全性无法保证。**比如一个线程在进行get然后put更新的操作，就是两个复合操作，在这两个操作之间，可能别的线程已经对这个key做了改动，接下来的put操作可能不符合预期。

##### Java集合中的快速失败（fast-fail）机制

快速失败是Java集合的一种**错误检测机制**，当多个线程对集合进行结构上的改变的操作时，**有可能**会产生fail-fast。

例如：

假设存在两个线程（线程1、线程2），线程1通过Iterator在遍历集合A中的元素，在某个时候线程2**修改了集合A的结构**（是结构上面的修改，而不是简单的修改集合元素的内容），那么这个时候程序就**可能**会抛出**ConcurrentModificationException**，从而产生fast-fail快速失败。

##### 快速失败机制的底层是怎么实现的？

迭代器在遍历时直接访问集合中的内容，并且在遍历过程中使用一个modCount变量。集合在被遍历期间如果内容发生变化，就会改变modCount的值。当迭代器使用hasNext()/next()遍历下一个元素之前，都会检测modCount变量是否为expectedModCount值，是的话就返回遍历；否则抛出异常，终止遍历。

##### ConcurrentModificationException

当检测到一个并发的修改，就可能会抛出该异常，一些迭代器的实现会抛出该异常，以便可以快速失败。但是不能为了便捷依赖该异常，而应该把它当作程序的政策手段。

#### HashMap的底层实现结构

JDK8之前，底层实现数据结构为**数组+链表**的形式，JDK8及以后的版本使用了**数组+链表+红黑树**实现，结局了链表太长导致查询速度变慢的问题。大概结构如下图：

![图片说明](https://raw.githubusercontent.com/juhick/picJuhick/master/20210421092228.png)

##### HashMap的初始容量、加载因子、扩容增量是多少？

HashMap的初始容量16，加载因子为0.75，扩容增量为原容量的1倍。如果HashMap的容量为16，一次扩容后容量为32。HashMap扩容是指元素个数**（包括数组和链表+红黑树中）**超过了16 * 0.75 = 12之后开始扩容。

##### 初始容量为什么是16？

如果太小，4或者8，**扩容比较频繁**；如果太大，32或者64甚至更大，**占用内存空间**

##### 默认加载因子为什么是0.75？

低了的话，比如0.5，那么哈希表填充到一半就开始扩容，这样会导致**扩容频繁**，并且空间利用率比较低。如果太大，比如1，就说明哈希表等到完全填满才开始扩容，这样虽然空间高度利用了，但是**哈希冲突**的机会也变大了，会提高查找成本。

##### 链表什么时候会变成红黑树，红黑树什么时候还原为链表

但添加元素时，如果桶中链表元素超过8，会自动转为红黑树。

当元素个数少于6时，树将还原为链表，设置为6的目的是为了防止链表和树之间频繁地切换。

##### 哈希表的最小树形化容量

最小树形化容量为64，但哈希表中的容量大于这个值时，表中的桶才能进行树形化

否则桶内元素太多时扩容，而不是树形化

##### HashMap的长度为什么是2的幂次方？

* 我们将一个键值对插入HashMap时，通过**将Key的hash值与length-1进行&运算**，实现了当前Key的定位，2的幂次方可以减少冲突（碰撞）的次数，提高HashMap查询效率
* **如果length为2的幂次方**，则length-1转化为二进制必定是11111……的形式，在与hash值的二进制与操作效率会非常快，而且空间不浪费
* **如果length不是2的幂次方**，比如length为15，则length-1为14，对应的二进制为1110，再与hash值进行与操作，最后一位都为0，而0001，0011，0101，0111，1001，1011，1101这几个位置永远都不能存放元素了，空间浪费相当大，更糟的是在这种情况中，数组可以使用的位置比数组长度小了很多，这意味着进一步增加了碰撞的几率，减慢了查询的效率！这要就会造成空间的浪费。

##### HashMap存储和获取原理

当**调用put()方法传递键和值来存储时**，先对键调用hashCode()方法，返回的hashCode用于**找到bucket位置来储存Entry对象**，也就是找到了该元素应该被存储的**桶中（数组）**。当两个键的hashCode值相同时，bucket位置发生了冲突，也就是**发生了Hash冲突**，这个时候，会在每一个bucket后边接上一个链表（JDK8及以后的版本中还会加上红黑树）来解决，将新存储的键值对放在表头（也就是bucket中）。

当调用get方法**获取存储的值**时，首先根据键的hashCode找到对应的bucket，然后根据equals方法来在链表和红黑树中找到对应的值。

##### HashMap的扩容步骤

HashMap里默认的负载因子大小为0.75.也就是说，当Map中的元素个数**（包括数组，链表和红黑树）**超过了16*0.75=12之后开始扩容。在准备树形化时发现数组长度太小也会进行扩容。将会创建原来HashMap大小的两倍的bucket数组，来重新调整map的大小，并将原来的对象放入新的bucket数组中。这个过程叫作**rehashing**，因为它调用hash方法找到新的bucket位置。

当然了，上述的扩容机制时比较低效的。所以在1.8版本时做了扩容效率方面的优化。因为2的N次幂扩容，所以一个元素要么在原位置不动，要么移动到当前位置+2的N次幂（也就是oldIndex+oldCap的未知）。

如何实现？

* 通过新增的bit位置上是0还是1来判断。
* 0是原位置，1则是oldIndex+oldCap的位置

该设计既省去了重新计算hash值的时间，同时，由于新增的1bit是0还是1可以认为是随机的，因此resize的过程，均匀地把之前冲突的节点分散到新的bucket中了。

##### 解决Hash冲突的方法有哪些？

* 拉链法**（HashMap使用的方法）**
* 线性探测再散列法
* 二次探测再散列法
* 伪随机探测再散列法

##### 哪些类适合作为HashMap的键？

String和Integer这样的包装类很适合作为HashMap的键，因为他们是final类型的类，而且**重写了equals和hashCode方法**，避免了键值对改写，有效提高HashMap性能。

为了计算hashCode()，就要防止键值改变，如果键值在放入时和获取时返回不同的hashCode的话，那么就不能从HashMap中找到想要的对象。

#### ConcurrentHashMap和Hashtable的区别

ConcurrentHashMap结合了HashMap和Hashtable的优势。HashMap没有考虑同步，Hashtable考虑了同步问题，但是Hashtable在每次同步执行时都要锁住整个结构。

ConcurrentHashMap锁的方式是稍微粒度的，ConcurrentHashMap将hash表分为16个桶（默认值），诸如get、put、remove等常用操作只锁上当前需要用到的桶。

##### ConcurrentHashMap的具体实现方式（分段锁）1.7及以前

* 该类包括两个**静态内部类MapEntry和Segment**,前者用来封装映射表的键值对，后者用来充当锁的角色。
* **Segment**是一种**可重入的锁ReentrantLock**，每个Segment守护一个HashEntry数组里的元素，当对HashEntry数组的数据进行修改时，必须首先获得对应的Segment锁。

在实际开发中，**单线程环境下可以使用HashMap，多线程环境下可以使用ConcurrentHashMap**。

#### 集合的类图

* HashMap的类图结构
  ![图片说明](https://raw.githubusercontent.com/juhick/picJuhick/master/20210421160909.png)
* ConcurrentHashMap的类图结构：
  ![图片说明](https://raw.githubusercontent.com/juhick/picJuhick/master/20210421160938.png)
* Hashtable的类图结构：
  ![图片说明](https://uploadfiles.nowcoder.com/images/20191026/5459305_1572062499403_8266E4BFEDA1BD42D8F9794EB4EA0A13)

#### TreeMap的特性

TreeMap底层使用**红黑树**实现，TreeMap中存储的键值对**按照键来排序**。

* 如果Key存入的时字符串等类型，那么会按照字典默认顺序排序

* 如果传入的是自定义引用类型，比如说User，那么该对象必须实现Comparable接口，并且覆盖其compareTo方法；或者在创建TreeMap的时候，我们必须指定使用多个比较器。如下所示：

  ```java
  // 方式一：定义该类的时候，就指定比较规则
  class User implements Comparable{
      @Override
      public int compareTo(Object o) {
          // 在这里边定义其比较规则
          return 0;
      }
  }
  public static void main(String[] args) {
      // 方式二：创建TreeMap的时候，可以指定比较规则
      new TreeMap<User, Integer>(new Comparator<User>() {
          @Override
          public int compare(User o1, User o2) {
              // 在这里边定义其比较规则
              return 0;
          }
      });
  }
  ```

##### Comparable和Comparator比较

**Comparable接口的后缀是able，表示可以的意思。**也就是说一个类如果实现了这个接口，那么这个**类就是可以比较的**。类似的还有cloneable接口表示可以克隆的。而**Comparator则是一个比较器**，是创建TreeMap的时候传入，用来指定比较规则。

- Comparable实现比较简单，但是当需要重新定义比较规则的时候，**必须修改源代码**，即修改User类里边的compareTo方法
- Comparator接口不需要修改源代码，只需要在创建TreeMap的时候**重新传入一个具有指定规则的比较器**即可。

#### ArrayList和LinkedList有哪些区别？

- ArrayList底层使用了**动态数组**实现，实质上是一个动态数组
- LinkedList底层使用了**双向链表**实现，可当作堆栈、队列、双端队列使用
- ArrayList在**随机存取**方面效率高于LinkedList
- LinkedList在节点的**增删方面**效率高于ArrayList
- ArrayList必须预留一定的空间，当空间不足的时候，会进行扩容操作
- LinkedList的开销是必须存储节点的信息以及节点的指针信息

还有一个集合**Vector**，它**是线程安全的ArrayList**，但是已经被废弃，不推荐使用了。多线程环境下，我们可以使用CopyOnWriteArrayList替代ArrayList来保证线程安全。

##### ArrayList的容量和扩容容量等

ArrayList默认的容量是10，默认最大容量是Interger.MAX_VALUE-8。

扩容核心源码如下：

```java
private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
```

其中参数是插入数据化所需的最小容量。

首先扩容是扩展原来的一半。若还不够直接扩展到所需容量。

若新的容量大于默认最大容量，则设置为Integer.MAX_VALUE。

#### HashSet和TreeSet有哪些区别？

- **HashSet底层使用了Hash表实现。**

保证元素唯一性的原理：判断元素的hashCode值是否相同。如果相同，还会继续判断元素的equals方法，是否为true

- **TreeSet底层使用了红黑树来实现。**

保证元素唯一性是通过Comparable或者Comparator接口实现

其实，HashSet的底层实现还是HashMap，只不过其只使用了其中的Key，具体如下所示：

- HashSet的add方法底层使用HashMap的put方法将key = e，value=PRESENT构建成key-value键值对，当此e存在于HashMap的key中，则value将会覆盖原有value，但是key保持不变，所以如果将一个已经存在的e元素添加中HashSet中，新添加的元素是不会保存到HashMap中，所以这就满足了HashSet中元素不会重复的特性。
- HashSet的contains方法使用HashMap得containsKey方法实现

#### LinkedHashMap和LinkedHashSet

**LinkedHashMap可以记录下元素的插入顺序和访问顺序**，具体实现如下：

- LinkedHashMap内部的Entry继承于HashMap.Node，这两个类都实现了Map.Entry<K,V>
- LinkedHashMap的Entry不光有value，next，还有before和after属性，这样通过一个双向链表，**保证了各个元素的插入顺序**
- 通过构造方法public LinkedHashMap(int initialCapacity,float loadFactor,boolean accessOrder)， **accessOrder传入true可以实现LRU缓存算法（访问顺序）**
- **LinkedHashSet 底层使用LinkedHashMap实现**，两者的关系类似与HashMap和HashSet的关系，大家可以自行类比。

##### 什么是LRU算法？LinkedHashMap如何实现LRU算法？

**LRU（Least recently used，最近最少使用）算法**根据数据的历史访问记录来进行淘汰数据，其核心思想是“如果数据最近被访问过，那么将来被访问的几率也更高”。

由于LinkedHashMap可以记录下Map中元素的访问顺序，所以可以轻易的实现LRU算法。只需要**将构造方法的accessOrder传入true，并且重写removeEldestEntry方法**即可。具体实现参考如下：

```cpp
import java.util.LinkedHashMap;
import java.util.Map;

public class LRUTest {

    private static int size = 5;

    public static void main(String[] args) {
        Map<String, String> map = new LinkedHashMap<String, String>(size, 0.75f, true) {
            @Override
            protected boolean removeEldestEntry(Map.Entry<String, String> eldest) {
                return size() > size;
            }
        };
        map.put("1", "1");
        map.put("2", "2");
        map.put("3", "3");
        map.put("4", "4");
        map.put("5", "5");
        System.out.println(map.toString());

        map.put("6", "6");
        System.out.println(map.toString());
        map.get("3");
        System.out.println(map.toString());
        map.put("7", "7");
        System.out.println(map.toString());
        map.get("5");
        System.out.println(map.toString());
    }
}
```

每次put后元素会放入最后边，每次访问后，被访问到的元素放到最后边，如果满了再插入的话，就会丢弃最前边的元素，新的插入最后边。

#### List和Set的区别

- List是有序的并且元素是**可以重复**的
- Set是无序（LinkedHashSet除外）的，并且元素是**不可以重复**的
  （此处的有序和无序是指**放入顺序和取出顺序**是否保持一致）

#### Iterator和ListIterator的区别

- Iterator可以遍历list和set集合；ListIterator只能用来遍历list集合
- Iterator只能前向遍历集合；ListIterator可以前向和后向遍历集合
- ListIterator其实就是实现了前者，并且增加了一些新的功能。

Iterator其实就是一个迭代器，在遍历集合的时候需要使用。Demo实现如下：

```java
        ArrayList<String> list =  new ArrayList<>();
        list.add("zhangsan");
        list.add("lisi");
        list.add("yangwenqiang");
        // 创建迭代器实现遍历集合
        Iterator<String> iterator = list.iterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        }
```

#### 数组和集合List之间的转换：

数组和集合List的转换在我们的日常开发中是很常见的一种操作，主要通过**Arrays.asList以及List.toArray方法**来搞定。这里给出Demo演示：

```java
package niuke;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class ConverTest {
    public static void main(String[] args) {
        // list集合转换成数组
        ArrayList<String> list =  new ArrayList<>();
        list.add("zhangsan");
        list.add("lisi");
        list.add("yangwenqiang");
        Object[] arr = list.toArray();
        for (int i = 0; i < arr.length; i++) {
            System.out.println(arr[i]);
        }
        System.out.println("---------------");
        // 数组转换为list集合
        String[] arr2 = {"niuke", "alibaba"};
        List<String> asList = Arrays.asList(arr2);
        for (int i = 0; i < asList.size(); i++) {
            System.out.println(asList.get(i));
        }

    }
}
```

##### 数组转为集合List：

通过Arrays.asList方法搞定，转换之后不可以使用add/remove等修改集合的相关方法，因为该方法返回的**其实是一个Arrays的内部私有的一个类ArrayList**，该类继承于Abstractlist，并没有实现这些操作方法，调用将会直接抛出UnsupportOperationException异常。这种转换体现的是一种**适配器模式**，只是转换接口，本质上还是一个数组。

##### 集合转换为数组：

List.toArray方法搞定了集合转换成数组，这里**最好传入一个类型一样的数组**，大小就是list.size()。因为如果入参分配的数组空间不够大时，toArray方法内部将重新分配内存空间，并返回新数组地址；如果数组元素个数大于实际所需，下标为list.size()及其之后的数组元素将被置为null，其它数组元素保持原值。所以，建议该方法入参数组的大小与集合元素个数保持一致。注意，需要传入包装类型数组，而不是基本类型数组。

若是**直接使用toArray无参方法**，此方法返回值只能是Object[ ]类，若强转其它类型数组将出现**ClassCastException**错误。

实例如下：

```java
ArrayList<String> list =  new ArrayList<>();
        list.add("zhangsan");
        list.add("lisi");
        list.add("yangwenqiang");
        String[] str = new String[3];
        list.toArray(str);
        for (String s : str) {
            System.out.println(s);
        }
```

#### Collection和Collections有什么关系

Collection是一个顶层集合接口，其子接口包括List和Set；而Collections是一个集合工具类，可以操作集合，比如说排序，二分查找，拷贝集合，寻找最大最小值等。 总而言之：带s的大都是工具类。

Collection是一个集合的顶层接口，Collections是进行集合操作的工具类。

Array是数组这个对象的类，Arrays是进行数组操作的工具类。

Executor是线程池的接口，Executors是进行线程池相关的操作的工具类。

#### 集合类图

* TreeMap的类图结构：
  ![图片说明](https://raw.githubusercontent.com/juhick/picJuhick/master/20210421213044.png)
* LinkedHashMap的类图结构：
  ![图片说明](https://raw.githubusercontent.com/juhick/picJuhick/master/20210421213119.png)
* ArrayList的类图结构：
  ![图片说明](https://raw.githubusercontent.com/juhick/picJuhick/master/20210421213158.png)
* LinkedList的类图结构：
  ![图片说明](https://raw.githubusercontent.com/juhick/picJuhick/master/20210421213407.png)
* Vector的类图结构：
  ![图片说明](https://raw.githubusercontent.com/juhick/picJuhick/master/20210421213433.png)
* HashSet的类图结构
  ![图片说明](https://raw.githubusercontent.com/juhick/picJuhick/master/20210421213457.png)
* TreeSet的类图结构：
  ![图片说明](https://raw.githubusercontent.com/juhick/picJuhick/master/20210421213532.png)