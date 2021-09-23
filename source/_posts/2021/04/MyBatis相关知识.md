---
title: MyBatis相关知识
date: 2021-04-24 09:03:09
tags:
 - 面试
 - MyBatis
categories:
 - 面试
math:
---

# MyBatis

### MyBatis是什么

MyBatis是一个**半ORM（对象关系映射）框架**，内部封装了JDBC，开发时只需要关注SQL语句本身，不需要花费精力去处理加载驱动、创建连接、创建statement等繁杂的过程。通过直接编写原生态SQL，可以严格控制SQL语句的执行性能，灵活度高(**支持动态SQL语句**)。

MyBatis **使用XML或注解来配置和映射原生信息**，将 POJO映射成数据库中的记录，避免了JDBC代码手动设置参数以及获取结果集的繁琐步骤。

MyBatis**通过xml文件或注解的方式将要执行的各种 statement 配置起来**，并通过Java对象和 statement中SQL的动态参数进行映射生成最终执行的SQL语句，最后由MyBatis框架执行SQL语句，并将结果映射为Java对象并返回。


<!-- more -->
#### MyBatis和Hibernate的区别

- **MyBatis**的优点是代码开发量少、简单易上手、SQL语句和代码分离（便于修改）、数据库可移植；但是，其缺点是SQL语句需要自己写，并且参数只能有一个。
- **Hibernate**的优点是进行了对象关系数据库映射、完全面向对象、提供缓存机制和HQL编程；但是，其缺点是不能灵活使用原生SQL、无法对SQL优化、全表映射效率。

#### JDBC、MyBatis和Hibernate的主要区别

JDBC更为灵活，更加有效率，系统运行速度快。但是代码繁琐复杂，如果使用了存储过程就不方便数据库移植了。

Hibernate和MyBatis是关系数据库框架，开发速度快，更加面向对象，可以移植更换数据库，但影响系统性能。

### MyBatis的核心组件

MyBatis的核心组件包括**SqlSessionFactoryBuilder，SqlSessionFactory，SqlSession和Mapper**。

**SqlSessionFactoryBuilder**是一个构建器，通过XML配置文件或者Java编码获得资源来构建SqlSessionFactory，通过Builder可以构建多个SessionFactory。其生命周期一般只存在于方法的局部，用完即可回收。

**SqlSessionFactory**的作用就是创建SqlSession，也就是创建一个会话。每次程序需要访问数据库，就需要使用到SqlSession。所以SqlSessionFactory应该在MyBatis应用的整个生命周期中。为了减少每次都创建一个会话带来的资源消耗，一般情况下都会使用单例模式来创建SqlSession。

**SqlSession**就是一个会话，相当于JDBC中的Connection对象，既可以发送SQL去执行并返回结果，也可以获取Mapper接口。SqlSession是一个线程不安全的对象，其生命周期应该是请求数据库处理事务的过程中。每次创建的SqlSession对象必须及时关闭，否则会使得数据库连接池的活动资源减少，影响系统性能。

**Mapper**也叫做映射器，由Java接口和XML文件（或者是注解）共同组成，给出了对应的SQL和映射规则，主要负责发送SQL去执行，并且返回结果。通过下边的语句来获取Mapper，接下来就可以调用Mapper中的各个方法去获取数据库结果了。

### 动态SQL

MyBatis动态SQL可以在XML映射文件内，以标签的形式编写动态SQL。动态SQL的执行原理是，**根据表达式的值完成逻辑判断并动态拼接SQL语句。**

对动态SQL的支持是MyBatis的一大优点。MyBatis提供了9种动态SQL标签：

- **if：**单条件分支的判断语句
- **choose, when, otherwise：**多条件的分支判断语句
- **foreach：**列举条件，遍历集合，实现循环语句
- **trim,where,set：**是一些辅助元素，可以对拼接的SQL进行处理
- **bind：**进行模糊匹配查询的时候使用，提高数据库的可移植性

### MyBatis的Mapper中常见标签

Mapper中常见的标签如下：

- select | insert | updae | delete
- resultMap | parameterMap | sql | include | selectKey
- trim | where | set | foreach | if | choose | when | otherwise | bind

### DAO接口

**Dao接口即Mapper接口**。接口的全限名，就是映射文件中的namespace的值。接口的方法名，就是映射文件中Mapper的Statement的id值。接口方法内的参数，就是传递给SQL的参数。

Mapper接口是没有实现类的，当调用接口方法时，接口**全限名+方法名**拼接字符串作为key值，可唯一定位一个**MapperStatement**。在MyBatis中，每一个`<select>`、`<insert>`、`<update>`、`<delete>`标签，都会被解析为一个**MapperStatement对象**。

Mapper接口的工作原理**是JDK动态代（dai）理**，MyBatis运行时会使用JDK动态代（dai）理为Mapper接口生成代（dai）理对象proxy，代（dai）理对象会拦截接口方法，转而执行MapperStatement所代表的SQL，然后将SQL执行结果返回。

#### Dao接口中的方法可以重载么？

**Mapper接口里的方法，是不能重载的，因为是使用 全限名+方法名 的保存和寻找策略。**

#### 不同映射文件xml中的id值可以重复么

- 不同的xml映射文件，如果配置了namespace，那么id可以重复
- 如果没有配置namespace，那么id不能重复

### `#`和`$`有什么区别

myBatis中我们**能用`#`号就尽量不要使用`$`符号**，它们的区别主要体现在下边几点：

- `#`符号将传入的数据都当做一个字符串，会对自动传入的数据加一个双引号
- `$`符号将传入的数据**直接显示**在生成的SQL语句中。
- `#`符号**存在预编译的过程**，对问号赋值，防止**SQL注入**。
- `$`符号是**直译**的方式，一般用在**order by ${列名}**语句中。

### MyBatis的缓存机制

MyBatis的缓存机制分为一级和二级缓存，分别介绍如下：

- **一级缓存（同一个SqlSession）:**

基于HashMap的本地缓存，其存储作用域为Session，当 Session flush 或 close 之后，该 Session 中的所有缓存就将清空，默认打开一级缓存。

- **二级缓存（同一个SqlSessionFactory）：**

二级缓存与一级缓存其机制相同，默认也是采用HashMap的本地存储，不同在于其存储作用域为 Mapper(Namespace)，并且可自定义存储源。

### MyBatis的接口绑定

接口绑定就是在MyBatis中任意定义接口,然后把接口里面的方法和SQL语句绑定, 我们直接调用接口方法就可以。

**接口绑定有两种实现方式：**

- 通过注解绑定，就是在接口的方法上面加上 @Select、@Update等注解，里面包含SQL语句来绑定
- 通过xml里面写SQL来绑定, 在这种情况下,要指定xml映射文件里面的namespace必须为接口的全路径名。

**接口绑定方式的选择：**

当SQL语句比较简单时候，用注解绑定；当SQL语句比较复杂时候，用xml绑定；实际的开发中，我们一般都使用xml绑定方式。