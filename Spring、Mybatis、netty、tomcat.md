# Spring

#### Spring是什么？

Spring是一个轻量级的控制反转(IoC)和面向切面(AOP)的容器框架。

控制反转——将对象的生成和管理交给IOC去实现，我们只需描述对象是如何生成的。

面向切面——将业务代码分离成切面，然后再插入到代码中进行内聚性开发。

[高内聚](https://www.baidu.com/s?wd=高内聚&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)，是指让一个类或者一个方法让他专注去做一件事情。低耦合：这个又要求对象，类之间减少[耦合性](https://www.baidu.com/s?wd=耦合性&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)，

#### BeanFactory：

BeanFactory是接口，给具体的IOC容器的实现提供了规范，它是负责创建和管理bean的一个工厂。

#### FactoryBean:

FactoryBean也是接口，主要用于返回BeanFactory生产好的bean对象，如果要获取FactoryBean对象，请在id前面加一个&符号来获取。

#### Spring使用如何解决线程安全问题?

1、在Controller中使用ThreadLocal变量
2、在spring配置文件Controller中声明 scope="prototype"，每次都创建新的controller

## SpringIoc

#### 什么是Spring IOC 容器？

Spring IOC 负责创建对象，管理对象，并且管理这些对象的整个生命周期。

#### IOC的优点是什么？

IOC 或 依赖注入把应用的代码量降到最低。IOC容器支持加载服务时的饿汉式初始化和懒加载。

#### ApplicationContext通常的实现是什么?

•	FileSystemXmlApplicationContext ：此容器从一个XML文件中加载beans的定义，XML Bean 配置文件的全路径名必须提供给它的构造函数。
•	ClassPathXmlApplicationContext：此容器也从一个XML文件中加载beans的定义，这里，你需要正确设置classpath因为这个容器将在classpath里找bean配置。
•	WebXmlApplicationContext：此容器加载一个XML文件，此文件定义了一个WEB应用的所有bean。

### 依赖注入

#### 什么是Spring的依赖注入？

a依赖于b，将b传入a就是依赖注入。

#### 有哪些不同类型的依赖注入方式？
a.接口注入
b.setter方法注入
c.构造方法注入

#### 哪种依赖注入方式你建议使用，构造器注入，还是 Setter方法注入？

单例模式下的setter循环依赖：通过“三级缓存”处理循环依赖。

### Spring Beans

#### spring是如何解决循环依赖：

1. 在finishBeanFactoryInitialization中，开始初始化A，毋庸置疑通过反射 
2. 之后A开始设置属性字段，此时发现需要一个B的对象。同时已标记A处于正在初始化阶段 
3. 显然接下来，开始去初始化B的对象，同样的手法，到设置属性阶段，发现需要A对象 
4. 于是乎，spring又开始去初始化对象A的依赖，此时先从缓存singletonObjects去取，没有再去看是否正处于初始阶段，是则再从缓存earlySingletonObjects中取，再没有，是则从singletonFactories三级缓存中取 
5. 将早期对象A设置到B中，再把B设置到A中
#### 解释Spring支持的几种bean的作用域。

Spring框架支持以下五种bean的作用域：
•	singleton : bean在每个Spring ioc 容器中只有一个实例。
•	prototype：一个bean的定义可以有多个实例。
•	request：每次http请求都会创建一个bean，该作用域仅在基于web的Spring ApplicationContext情形下有效。
•	session：在一个HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。
•	global-session：在一个全局的HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。
缺省的Spring bean 的作用域是Singleton.

#### Spring框架中的单例bean是线程安全的吗?

不，Spring框架中的单例bean不是线程安全的。

#### 解释Spring框架中bean的生命周期

![beanå®ä¾åè¿ç¨](https://blog-pictures.oss-cn-shanghai.aliyuncs.com/bean%e5%ae%9e%e4%be%8b%e5%8c%96%e8%bf%87%e7%a8%8b.png)

#### spring容器的初始化过程：

读取xml用Resource封装，将Resource解析成BeanDefinition，创建BeanFactory来装配BeanDefiniton，加工处理BeanDefiniton，注册了bean后处理器，初始化资源、广播器，然后注册事件监听器，初始化所有singleton的bean，最后发布上下文刷新事件

## SpringAop

#### 解释AOP:

面向切面的编程，或AOP，是一种编程技术，允许程序模块化横向切割关注点，或横切典型的责任划分，如日志和事务管理。

#### Aspect 切面

AOP核心就是切面，它将多个类的通用行为封装成可重用的模块，该模块含有一组API提供横切功能。比如，一个日志模块可以被称作日志的AOP切面。根据需求的不同，一个应用程序可以有若干切面。在Spring AOP中，切面通过带有@Aspect注解的类实现。

#### Join Point连接点

连接点代表一个应用程序的某个位置，在这个位置我们可以插入一个AOP切面，它实际上是个应用程序执行Spring AOP的位置。

#### Advice通知

通知是个在方法执行前或执行后要做的动作，实际上是程序执行时要通过SpringAOP框架触发的代码段。

#### Point Cut切点

切入点是一个或一组连接点，通知将在这些位置执行。可以通过表达式或匹配的方式指明切入点。

![è¿éåå¾çæè¿°](https://img-blog.csdn.net/20180530175605692?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3E5ODIxNTE3NTY=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### jdk动态代理和cglib动态代理区别：

一、简单来说：

　　**JDK动态代理只能对实现了接口的类生成代理，而不能针对类**

　　**CGLIB是针对类实现代理，主要是对指定的类生成一个子类，覆盖其中的方法（继承）**

二、Spring在选择用JDK还是CGLiB的依据：

   (1)当Bean实现接口时，Spring就会用JDK的动态代理

   (2)当Bean没有实现接口时，Spring使用CGlib是实现

主要区别：cglib会对代码侵入，jdk不会对代码侵入，便于维护代码

## 自定义注解

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface AnalysisActuator {
}
```
```java
@Aspect
@Component
public class AnalysisActuatorAspect {
    @Pointcut("@annotation(analysisActuator)")
    public void serviceStatistics(AnalysisActuator analysisActuator) {
    }

    @Before("serviceStatistics(analysisActuator)")
    public void doBefore(JoinPoint joinPoint, AnalysisActuator analysisActuator) {
	......
    }
    
    @After("serviceStatistics(analysisActuator)")
    public void doAfter(AnalysisActuator analysisActuator) {
    ......
    }

}
```

## Spring事务管理

#### Spring支持两种方式的事务管理：

编程式事务管理： 通过Transaction Template手动管理事务，实际应用中很少使用，
使用XML配置声明式事务： 推荐使用（代码侵入性最小），实际是通过AOP实现

#### 实现声明式事务的方式：

**基于 @Transactional 的全注解方式：** 将声明式事务管理简化到了极致。开发人员只需在配置文件中加上一行启用相关后处理 Bean 的配置，然后在需要实施事务管理的方法或者类上使用 @Transactional 指定事务规则即可实现事务管理，而且功能也不比其他方式逊色。

**@Transactional**(propagation = Propagation.REQUIRED, isolation = Isolation.DEFAULT, readOnly = **false**, timeout = -1) （Isolation设置隔离级别）

#### Spring事务传播机制：

PROPAGATION_REQUIRED
有事务就用已有的，没有就重新开启一个

PROPAGATION_SUPPORTS
有事务就用已有的，没有也不会重新开启

PROPAGATION_MANDATORY
必须要有事务，没事务抛异常

PROPAGATION_REQUIRES_NEW
开启新事务，若当前已有事务，挂起当前事务

PROPAGATION_NOT_SUPPORTED
不需要事务，若当前已有事务，挂起当前事务

PROPAGATION_NEVER
不需要事务，若当前已有事务，抛出异常

PROPAGATION_NESTED
嵌套事务，如果外部事务回滚，则嵌套事务也会回滚！！！外部事务提交的时候，嵌套它才会被提交。嵌套事务回滚不会影响外部事务。

## Spring注解

@RequestParam：用于将请求参数区数据映射到功能处理方法的参数上。

@PathVariable：用于将请求URL中的模板变量映射到功能处理方法的参数上。

@Responsebody：表示该方法的返回结果直接写入HTTP response body中。

@Param：映射器的方法需要多个参数

## SpingMVC

#### MVC框架

![img](https://gss3.bdstatic.com/-Po3dSag_xI4khGkpoWK1HF6hhy/baike/c0%3Dbaike80%2C5%2C5%2C80%2C26/sign=7948cf4dbf096b63951456026d5aec21/b03533fa828ba61edbddc04d4034970a304e59a4.jpg)

#### 什么是Spring的MVC框架？

Spring 配备构建Web 应用的全功能MVC框架。Spring可以很便捷地和其他MVC框架集成，如Struts，Spring 的MVC框架用控制反转把业务对象和控制逻辑清晰地隔离。它也允许以声明的方式把请求参数和业务对象绑定。

![SpringMVCè¿è¡åç](https://camo.githubusercontent.com/6889f839138de730fce5f6a0d64e33258a2cf9b5/687474703a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f31382d31302d31312f34393739303238382e6a7067)

#### Spring和SpringMVC的关系
那么SpringMVC子容器用来注册web组件的Bean，如控制器、处理器映射、视图解析器等。而Spring用来注册其他Bean，这些Bean通常是驱动应用后端的中间层和数据层组件。

#### 为什么要使用springmvc

要使用Servlet完成的复杂的功能时，需要编写多个Servlet类，并且在web.xml进行注册

Spring web MVC框架提供了MVC架构和用于开发灵活Web应用程序

比起Servlet，更好更简单地接受参数；

更简单地处理返回对象数据；

# Mybatis

#### Mybatis缓存：

**一级缓存的作用域是同一个SqlSession**，在同一个sqlSession中两次执行相同的sql语句，第一次执行完毕会将数据库中查询的数据写到缓存（内存），第二次会从缓存中获取数据将不再从数据库查询，从而提高查询效率。当一个sqlSession结束后该sqlSession中的一级缓存也就不存在了。Mybatis**默认开启一级缓存**。

spring整合mybatis后，非事务环境下，每次操作数据库都使用新的sqlSession对象。因此mybatis的一级缓存无法使用

**二级缓存是mapper级别的缓存**，多个SqlSession去操作同一个Mapper的sql语句，多个SqlSession去操作数据库得到数据会存在二级缓存区域，多个SqlSession可以共用二级缓存，二级缓存是跨SqlSession的。不同的sqlSession两次执行相同namespace下的sql语句且向sql中传递参数也相同即最终执行相同的sql语句，第一次执行完毕会将数据库中查询的数据写到缓存（内存），第二次会从缓存中获取数据将不再从数据库查询，从而提高查询效率。Mybatis默认没有开启二级缓存需要在setting全局参数中配置开启二级缓存

#### resultmap和resultType区别：

resultType：使用resultType进行输出映射，只有查询出来的列名和pojo（实体bean）中的属性名一致，该列才可以映射成功。

如果查询出来的列名和pojo中的属性名全部不一致，没有创建pojo对象。
只要查询出来的列名和pojo中的属性有一个一致，就会创建pojo对象。

resultMap：如果查询出来的列名和pojo的属性名不一致，通过定义一个resultMap对列名和pojo属性名之间作一个映射关系。

#### MyBatis中#{}和${}区别：

- \#{}''是经过预编译的,是安全的,而${}是未经过预编译的,仅仅是取变量的值,是非安全的,存在sql注入。
- 只能''＄{}''的情况,order by、like 语句只能用＄{}了,用#{}会多个' '导致sql语句失效.此外动态拼接sql也要用''${}''
- ''#{}'' 这种取值是编译好SQL语句再取值, ${} 这种是取值以后再去编译SQL语句

重要：接受从用户输出的内容并提供给语句中不变的字符串，这样做是不安全的。这会导致潜在的sql注入攻击，因此你不应该允许用户输入这些字段，或者通常自行转义并检查。

#### mybatis和hibernate的区别

1.hibernate需要设置映射关系，mybatis不用

2.hibernate是全自动，mybatis是半自动，MyBatis可以进行更为细致的SQL优化，可以减少查询字段，对查询进行优化。

3.MyBatis容易掌握，而Hibernate门槛较高。

4.hibernate支持第三方缓存。

# SpringBoot

#### 什么是springboot

1.用来简化spring应用的初始搭建以及开发过程 使用特定的方式来进行配置（properties或yml文件）实现无xml
2.嵌入的Tomcat 无需部署war文件
3.尽可能自动配置Spring，springmvc和第三方库

####  springboot自动配置的原理

Spring Boot启动扫描所有jar包的META-INF/spring.factories中配置的 EnableAutoConfiguration组件 ,然后自动配置类根据@ConditionalOnClass找出哪些类需要自动配置，然后@ConditionalOnMissingBean看否有已经生成好的bean，有的化则不需要自动配置类去生产了；

#### 初始化流程

1.通过 `SpringFactoriesLoader` 加载 `META-INF/spring.factories` 文件，获取并创建 `SpringApplicationRunListener` 对象

2.然后由 `SpringApplicationRunListener` 来发出 starting 消息

3.创建参数，并配置当前 SpringBoot 应用将要使用的 Environment

4.完成之后，依然由 `SpringApplicationRunListener` 来发出 environmentPrepared 消息

5.初始化applicationContext后，继续由 `SpringApplicationRunListener` 来发出 contextLoaded 消息，告知 SpringBoot 应用使用的 `ApplicationContext` 已装填OK

6.refresh ApplicationContext，完成IoC容器可用的最后一步

7.由 `SpringApplicationRunListener` 来发出 started 消息

8.完成最终的程序的启动

9.由 `SpringApplicationRunListener` 来发出 running 消息，告知程序已运行起来了

![SpringBoot 应用启动流程图](https://user-gold-cdn.xitu.io/2018/9/5/165a6ae37ed44681?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

# Tomcat

#### tomcat架构

（1）Tomcat中只有一个Server，一个Server可以有多个Service，一个Service可以有多个Connector和一个Container； 
（2）Server掌管着整个Tomcat的生死大权； 
（3）Service 是对外提供服务的； 
（4）Connector用于接受请求并将请求封装成Request和Response来具体处理； 
（5）Container用于封装和管理Servlet，以及具体处理request请求；

#### tomcat优化

**一:Tomcat 内存优化,启动时告诉JVM我要一块大内存(调优内存是最直接的方式)**

**二:Tomcat 线程优化** 在server.xml中 如:

```
<Connector port="80" protocol="HTTP/1.1" maxThreads="600" minSpareThreads="100" maxSpareThreads="500" acceptCount="700"
connectionTimeout="20000"  />
```

maxThreads="X" 表示最多同时处理X个连接

minSpareThreads="X" 初始化X个连接

maxSpareThreads="X" 表示如果最多可以有X个线程，一旦超过X个,则会关闭不在需要的线程

acceptCount="X" 当同时连接的人数达到maxThreads时,还可以排队,队列大小为X.超过X就不处理

**三:Tomcat IO优化**

在server.xml中修改protocol实现（APR,BIO,NIO,AIO）

# Netty

## Netty:高性能的通信框架

1.使用java nio(nio和io多路复用)技术,支持高并发

2.传输快，零拷贝技术；

3.使用主从Reactor多线程模型

4.默认使用Protobuf的序列化框架

#### Netty 中有那种重要组件？

- ChannelPipeline：为 ChannelHandler 链提供了容器，当 channel 创建时，就会被自动分配到它专属的 ChannelPipeline，这个关联是永久性的。
- Channel：Netty 网络操作抽象类，它除了包括基本的 I/O 操作，如 bind、connect、read、write 等。
- ChannelHandler：充当了所有处理入站和出站数据的逻辑容器。ChannelHandler 主要用来处理各种事件，这里的事件很广泛，比如可以是连接、数据接收、异常、数据转换等。

#### 零拷贝： 

1.Netty的接收和发送数据采用DIRECT BUFFERS，使用堆外直接内存进行Socket读写，不用在字节缓冲区的进行二次拷贝，如果使用传统的堆内存（HEAP BUFFERS）进行 Socket 读写，JVM 会将堆内存 Buffer 拷贝一份到直接内存中，然后才写入 Socket 中。

2.Netty有组合Buffer对象，可以一次性对多个ByteBuffer对象操作 

3.Netty的文件传输采用了transferTo方法，它可以直接将文件缓冲区的数据发送到目标Channel，不用传统通过循环write方式

![img](https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=3368301146,3506362174&fm=26&gp=0.jpg)

![img](https://ss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=3153115758,3045316412&fm=26&gp=0.jpg)

#### 内存池：

频繁的申请和释放内存对GC不友好，因此Netty开发了一个内存池模块用于内存的分配和回收来降低GC压力，也就是直接内存。

#### 常见的Reactor线程模型：

Reactor单线程模型；指的是所有的I/O操作和建立tcp都在同一个NIO线程上面完成，
Reactor多线程模型；有一组NIO线程处理I/O操作
主从Reactor多线程模型；服务端用于接收客户端连接的不再是1个单独的NIO线程，而是一个独立的NIO线程池。

![è¿éåå¾çæè¿°](https://img-blog.csdn.net/20180523184528107?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1bjc1NDU1MjY=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

