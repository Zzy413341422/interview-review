# Jvm虚拟机

## 介绍下Java内存区域

Java堆：占据了虚拟机管理内存中最大的一块，唯一目的就是存放对象实例，也是垃圾回收器主要管理的地方 

方法区（1.7）->元空间（1.8）：它用于存储已被虚拟机加载的类信息、常量、静态变量。元空间与永久代之间**最大的区别在于：元空间并不在虚拟机中，而是使用本地内存**。

程序计数器:循环、跳转、异常处理、线程恢复等功能都需要依赖这个计数器来完。

Java 栈:局部变显表:存基本数据类型.对象的引用, returnAddress类型.。

本地方法栈:为虚拟机使用到的 Native 方法服务。

各对象存放位置：

Java6和6之前，常量池是存放在方法区（永久代）中的。

Java7，将常量池是存放到了堆中。

Java8之后，取消了整个永久代区域。运行时常量池和静态常量池存放在元空间中，而字符串常量池依然存放在堆中。


```java
  		  String str1 = "str";
  		  String str2 = "ing";
  		  String str3 = "str" + "ing";//常量池中的对象
  		  String str4 = str1 + str2; //在堆上创建的新的对象	  
  		  String str5 = "string";//常量池中的对象
  		  System.out.println(str3 == str4);//false
  		  System.out.println(str3 == str5);//true
  		  System.out.println(str4 == str5);//false
```

## 是不是所有的对象和数组都会在堆内存分配空间？

不一定，随着JIT编译器的发展，在编译期间，如果JIT经过逃逸分析，发现有些对象没有逃逸出方法，那么有可能堆内存分配会被优化成栈内存分配。但是这也并不是绝对的。就像我们前面看到的一样，在开启逃逸分析之后，也并不是所有User对象都没有在堆上分配。

## 如何判断对象是否死亡（两种方法）。

引用计数法: 给对象中添加一个引用计数器，每当有一个地方引用它，计数器就加1；当引用失效，计数器就减1；任何时候计数器为0的对象就是不可能再被使用的。缺点：互相引用造成回收失败
可达性分析算法:通过一系列的称为 “GC Roots” 的对象作为起点,当一个对象到GC Roots没有任何引用链相连的话，则证明此对象是不可用的。

GC Roots：

1.栈中引用的对象；

2.方法区中的静态，常量引用对象；

3.本地方法栈中引用的对象；

## 判断对象死亡后，如何避免GC

![img](md\22.png)

## Stop-The-World原因

java线程要停止才能彻底清除干净，否则会影响gc线程的处理效率增加gc线程负担

## 简单的介绍一下强引用、软引用、弱引用、虚引用（虚引用与软引用和弱引用的区别、使用软引用能带来的好处）。

#### 1．强引用

垃圾回收器绝不会回收它。当内存空间不足，Java虚拟机宁愿抛出OutOfMemoryError错误，使程序异常终止，也不会靠随意回收具有强引用的对象来解决内存不足问题。

#### 2．软引用（SoftReference）

如果内存空间足够，垃圾回收器就不会回收它，如果内存空间不足了，就会回收这些对象的内存。只要垃圾回收器没有回收它，该对象就可以被程序使用。软引用可用来实现内存敏感的高速缓存。

#### 3．弱引用（WeakReference）

一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存。

#### 4．虚引用（PhantomReference）

"虚引用"顾名思义，就是形同虚设，与其他几种引用都不同，虚引用并不会决定对象的生命周期。如果一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收。

## 垃圾收集有哪些算法，各自的特点？

标记-清除算法：效率问题 + 空间问题
复制算法：占用空间大
标记-整理算法：所有存活的对象向一端移动，然后直接清理掉端边界以外的内存。

## 并行和并发：

收集器中的并行：指多条垃圾收集线程并行工作，但此时用户线程仍处于等待状态
收集器中的并发：指用户线程与垃圾收集线程同时执行（但不一定是并行的，可能会交替执行），用户程序在继续运行，而垃圾收集程序于另一个CPU上。

## 介绍一下收集器。

![img](md\23.png)

•  Serial收集器（复制算法): 新生代单线程收集器，标记和清理都是单线程，优点是简单高效； 
•  Serial Old收集器 (标记-整理算法): 老年代单线程收集器，Serial收集器的老年代版本； 
•  ParNew收集器 (复制算法):新生代并行收集器，实际上是Serial收集器的多线程版本，在多核CPU环境下有着比Serial更好的表现； 
•  Parallel Scavenge收集器 (复制算法): 新生代并行收集器，追求高吞吐量，高效利用 CPU。
•  Parallel Old收集器 (标记-整理算法)： 老年代并行收集器，吞吐量优先，Parallel Scavenge收集器的老年代版本； 
•  CMS(Concurrent Mark Sweep)收集器（标记-清除算法）：老年代并行收集器，以获取最短回收停顿时间为目标的收集器，具有高并发、低停顿的特点，追求最短GC回收停顿时间。 
•  G1(Garbage First)收集器 (整体上是基于标记-整理 ，局部采用复制)：Java堆并行收集器，G1收集器是JDK1.7提供的一个新收集器，G1收集器基于“标记-整理”算法实现，也就是说不会产生内存碎片。此外，G1收集器不同于之前的收集器的一个重要特点是：G1回收的范围是整个Java堆(包括新生代，老年代)，而前六种收集器回收的范围仅限于新生代或老年代。

jdk8默认Parallel Scavenge + Serial Old jdk9默认g1

## 谈谈你对 CMS 垃圾收集器的理解？

#### Serial单线程垃圾回收器

![img](md\126.png)

#### Parallel 并行收集器

![img](md\125.png)

#### CMS收集器

初始标记（CMS initial mark）：仅仅只是标记一下GC Roots能直接关联到的对象，速度很快，需要“Stop The World”。

并发标记（CMS concurrent mark）：进行GC Roots Tracing的过程，在整个过程中耗时最长。

重新标记（CMS remark）：为了修正并发标记期间因用户程序继续运作而导致标记产生变动的那一部分对象的标记记录，这个阶段的停顿时间一般会比初始标记阶段稍长一些，但远比并发标记的时间短。此阶段也需要“Stop The World”。

并发清除（CMS concurrent sweep）

![img](md\80.png)

缺点：标记-清除算法导致的空间碎片，可以配置n次Full GC后压缩空间

浮动垃圾：由于CMS并发清理阶段用户线程还在运行着，伴随程序运行自然就还会有新的垃圾不断产生。

并发阶段不停顿，但会占用CPU

#### G1收集器

初始标记（Initial Marking） 仅仅只是标记一下GC Roots 能直接关联到的对象，并且修改TAMS（Nest Top Mark Start）的值，让下一阶段用户程序并发运行时，能在正确可以的Region中创建对象，此阶段需要停顿线程，但耗时很短。

并发标记（Concurrent Marking） 从GC Root 开始对堆中对象进行可达性分析，找到存活对象，此阶段耗时较长，但可与用户程序并发执行。

最终标记（Final Marking） 为了修正在并发标记期间因用户程序继续运作而导致标记产生变动的那一部分标记记录，虚拟机将这段时间对象变化记录在线程的Remembered Set Logs里面，最终标记阶段需要把Remembered Set Logs的数据合并到Remembered Set中，这阶段需要停顿线程，但是可并行执行。

筛选回收（Live Data Counting and Evacuation） 首先对各个Region中的回收价值和成本进行排序，根据用户所期望的GC 停顿是时间来制定回收计划。此阶段其实也可以做到与用户程序一起并发执行，但是因为只回收一部分Region，时间是用户可控制的，而且停顿用户线程将大幅度提高收集效率。

![img](md\81.png)

（1）并行与并发：能充分利用多CPU，多核环境下的硬件优势。

（2）分代收集：分代概念在G1中依然得以保留。G1可以不需要其他收集配合就能独立管理整个GC堆，但它能够采用不同的方式去处理新创建的对象和已经存活了一段时间、熬过多次GC的旧对象以获取更好的收集效果。

（3）空间整合：G1从整体上看是基于“标记 - 整理”算法实现的收集器，从局部（两个Region之间）上来看是基于“复制”算法。都不会产生内存空间碎片。

（4）可预测的停顿：降低停顿时间是G1和CMS共同的关注点，但G1除了追求低停顿外，还能建立可预测的停顿时间模型。能让使用者明确指定在一个长度为M毫秒的时间片段内，消耗在垃圾收集上的时间不得超过N毫秒。

## 什么对象进入老年代：

1.大对象；

2.长期存活的对象；

3.相同年龄的对象占了survivor空间的一半

![img](C:/Users/admin/Desktop/interview-review/md/24.png)

## 什么时候触发MinorGC?什么时候触发FullGC?

Minor GC：

当Eden区和From Survivor区满时；

Full GC：

调用System.gc时，系统建议执行Full GC，但是不必然执行

老年代空间不足

方法区空间不足

进入老年代对象的大小大于老年代的可用内存

## 一个类的生命周期

编译，即把我们写好的java文件，通过javac命令编译成字节码，也就是我们常说的.class文件。
运行，则是把编译生成的.class文件交给Java虚拟机(JVM)执行。
举个通俗点的例子来说，JVM在执行某段代码时，遇到了class A， 然而此时内存中并没有class A的相关信息，于是JVM就会到相应的class文件中去寻找class A的类信息，并加载进内存中，这就是我们所说的类加载过程。
由此可见，JVM不是一开始就把所有的类都加载进内存中，而是只有第一次遇到某个需要运行的类时才会加载，且只加载一次。

## 简单说说类加载过程，里面执行了哪些操作？

![ç±»å è½½è¿ç¨-11.2kB](md\25.png)

在使用前加载到JVM

**类型的加载**：查找并加载类的二进制数据（字节码文件），最常见的，是将类的Class文件从磁盘加载到内存中。

**类型的连接**：将类与类的关系确定好，对于字节码相关的处理、验证、校验在加载连接阶段去完成的。字节码本身可以被人为操纵的，也因此可能有恶意的可能性，所以需要校验。

- **验证：**确保被加载类的正确性，就是要按照JVM规范定义的。

- **准备：**为类的静态变量分配内存，并将其初始化为默认值

  如下代码所示，中间过程，在将类型加载到内存过程中，num分配内存，首先设置为0，1是在后续的初始化阶段赋值给num变量

  ```
  class Test{   public static int num = 1;  }
  ```

- **解析：**把类中的符号引用转换为直接引用 **符号引用：** 

**类型的初始化**：比如一些静态的变量的赋值是在初始化阶段完成的。

## 类的实例化与类的初始化

- 类的实例化是指创建一个类的实例(对象)的过程；
- 类会在首次被“主动使用”时执行初始化，类的初始化是指**实例变量初始化**、**实例代码块初始化** 以及 **构造函数初始化**，是类生命周期中的一个阶段。准备阶段，JVM为[静态变量](https://www.baidu.com/s?wd=静态变量&tn=44039180_cpr&fenlei=mv6quAkxTZn0IZRqIHckPjm4nH00T1Y4PHRdPj6Ym1TsnAPhrH6d0ZwV5Hcvrjm3rH6sPfKWUMw85HfYnjn4nH6sgvPsT6KdThsqpZwYTjCEQLGCpyw9Uz4Bmy-bIi4WUvYETgN-TLwGUv3EnHbvnjbvnHDdPW0vnj6sPHfvn0)[分配内存](https://www.baidu.com/s?wd=分配内存&tn=44039180_cpr&fenlei=mv6quAkxTZn0IZRqIHckPjm4nH00T1Y4PHRdPj6Ym1TsnAPhrH6d0ZwV5Hcvrjm3rH6sPfKWUMw85HfYnjn4nH6sgvPsT6KdThsqpZwYTjCEQLGCpyw9Uz4Bmy-bIi4WUvYETgN-TLwGUv3EnHbvnjbvnHDdPW0vnj6sPHfvn0)空间

## 对类加载器有了解吗？什么是双亲委派模型？

![ClassLoader](md\26.png)

## 不同的类加载器在方法区有对应相对隔离的空间

## 双亲委派模型

BootstrapClassLoader：Java类加载层次中最顶层的类加载器，负责加载JDK中的核心类库

ExtClassLoader称为扩展类加载器，主要负责加载Java的扩展类库,默认加载JAVA_HOME/jre/lib/ext/目录下的所有jar包

AppClassLoader应用类加载器,又称为系统类加载器,负责在JVM启动时,加载来自在命令java中的classpath的jar包

## 如何破坏双亲委派模型

打破双亲委派机制则不仅要继承ClassLoader类，还要重写loadClass和findClass方法

沿用双亲委派机制自定义类加载器很简单，只需继承ClassLoader类并重写findClass方法即可。

## 热部署为什么要自定义类加载器

class文件被修改过，那么先卸载对应class类加载器，重新启动，然后就会重新加载类。（你不可能重新调用B,E,A类加载器）。

## tomcat为什么要自定义类加载器

一个web容器可能需要部署两个应用程序，不同的应用程序可能会依赖同一个第三方类库的不同版本，如果使用默认的类加载器机制，那么是无法加载两个相同类库的不同版本的，由全限定类名决定，并且只有一份。

web容器也有自己依赖的类库，不能于应用程序的类库混淆。

#### 调优工具：

jstack：通过 jstack 命令可以获取当前进程的所有线程信息，Java进程有多少线程，每个线程什么状态，是不是在等着锁

jps：显示当前所有java进程pid的命令

Jstat：对其堆的使用情况进行实时的命令行的统计

Jconsole：可以以图表化的形式显示各种数据

jvm内存大小如何设置 https://www.cnblogs.com/zhangfengshi/p/11343102.html

xmx=xms：避免每次动态扩容都都必须执行一次Full GC

### 如何排查cpu100%

top -> top -Hp PID ->（找到最高的线程26045） jstack -l 26045 > ./26045.stack  ->（线程id转为16进制65be）  -> cat 26045.stack ｜ grep '65be' -C 20

**1、**频繁的GC; 如果访问量很高，可能会导致频繁的GC甚至FGC。当调用量很大时，内存分配将如此之快以至于GC线程将连续执行，这将导致CPU飙升。

**3、**序列化和反序列化，md5等; 

**4、**正则表达式。我遇到了正则表达式使CPU充满的情况; 原因可能是Java正则表达式使用的引擎实现是NFA自动机，它将在字符匹配期间执行回溯。 

**5、**线程上下文切换; 有许多已启动的线程，这些线程的状态在Blocked（锁定等待，IO等待等）和Running之间发生变化。当锁争用激烈时，这种情况很容易发生。 

**6、**有些线程正在执行非阻塞操作，例如 `while(true)`语句。如果在程序中计算需要很长时间，则可以使线程休眠。

### 如何查找内存泄漏

将进程堆的情况jmap -dump下来，使用分析工具进行分析，找到发生内存占用过大的对象。

## OOM原因

原来当一个线程抛出OOM异常后，它所占据的内存资源会全部被释放掉，从而不会影响其他线程的运行！

#### java.lang.OutOfMemoryError: Java heap space

#### java.lang.OutOfMemoryError: GC overhead limit exceeded

同上

#### java.lang.OutOfMemoryError: unable to create new native thread

不断地建立线程的方式导致内存溢出。

#### java.lang.OutOfMemoryError: Metaspace

运行时产生大量的类

#### java.lang.OutOfMemoryError: Direct buffer memory

本机直接内存溢出

## JVM-Linux

问题排查：

oom 问题排查思路 

线上问题排查，突然长时间未响应，怎么排查，oom 

cpu 使用率特别高，怎么排查？通用方法？定位代码？cpu高的原因？ 

频繁 GC 原因？什么时候触发 FGC？ 

怎么获取 dump 文件？怎么分析？

15、代码中遇到进程阻塞，进程僵死，内存泄漏等情况怎么排查。通过ps查询状态，分析dump文件等方式排查。 

 16、Linux了解么，查看进程状态ps，查看cpu状态 top。查看占用端口的进程号netstat grep

8、Linux大文件怎么查某一行的内容。

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

#### BeanFactory和ApplicationContext的区别是什么：

BeanFactory负责读取bean配置文档，管理bean的加载，实例化，维护bean之间的依赖关系，负责bean的声明周期。

#### Spring使用如何解决线程安全问题?

1、在Controller中使用ThreadLocal变量
2、在spring配置文件Controller中声明 scope="prototype"，每次都创建新的controller

## SpringIoc

#### 什么是Spring IOC 容器？

Spring IOC 负责创建对象，管理对象，并且管理这些对象的整个生命周期。定位 加载 解析和注册

#### IOC的优点是什么？

IOC 或 依赖注入把应用的代码量降到最低。IOC容器支持加载服务时的饿汉式初始化和懒加载。

#### SpringIOC的优点？

1. 对象提供了n个构造器, 实例化如何选择构造器? 构造器内所需参数也是bean, 该如何处理? 在beanDefinition中定义了构造器的参数, 构造器又该怎么选择?(AutowiredAnnotationBeanPostProcessor#determineCandidateConstructors了解下>
2. 对象间需要循环依赖怎么处理? 当循环依赖的对象又都需要被代理, 又怎么处理? 当这些对象生成后客户需要继续对这些对象再处理又该怎么处理? 
3. @Configuration标注的配置类如何避免重复创建对象? Spring中各种关系怎么处理?[@Conditional, @DependsOn, @Import(import其他后置处理器又该怎么处理>]
4. 用户需要自定义bean到容器里, 如何提供扩展? 允许bean整个生命周期进行干涉, 又如何扩展?([@Autowired, @Resource]注解也只是扩展, 源码里删掉处理@Autowired的后置处理器照样跑>
5. 允许用户直接设置bean属性, 值可以设置任意对象, 当所需值是[class, bean, 原型bean, 集合容器, 泛型, File, Resource]时如何处理?

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
d.注解注入：基于反射原理

#### 哪种依赖注入方式你建议使用，构造器注入，还是 Setter方法注入？

单例模式下的setter循环依赖：通过“三级缓存”处理循环依赖。

### Spring Beans

#### spring是如何解决循环依赖：

![](md\87.png)

    protected Object getSingleton(String beanName, boolean allowEarlyReference) {
        // 1. 尝试去一级缓存中加载我们的bean，IOC容器初始化加载单例bean的时候，第一次进来都会返回null，一级缓存保存的已经处理完成的对象
        Object singletonObject = this.singletonObjects.get(beanName);
        if (singletonObject == null && this.isSingletonCurrentlyInCreation(beanName)) {
            synchronized(this.singletonObjects) {
                // 2. 一级缓存找不到从二级缓存中查找
                singletonObject = this.earlySingletonObjects.get(beanName);
                if (singletonObject == null && allowEarlyReference) {
                    // 3. 二级缓存中找不到去三级缓存中查
                    ObjectFactory<?> singletonFactory = (ObjectFactory)this.singletonFactories.get(beanName);
                    if (singletonFactory != null) {
                        singletonObject = singletonFactory.getObject();
                        // 获取的还没实例化完成的单例bean会被放入二级缓存
                        this.earlySingletonObjects.put(beanName, singletonObject);		
                        // 移除三级缓存中指定的bean
                        this.singletonFactories.remove(beanName);
                    }
                }
            }
        }
        return singletonObject;
    }
###### 第二级缓存earlySingletonObjects的作用

假如二级缓存存放工厂，这样每次都会重新生成对象，假如二级缓存存放未初始化的bean，这样每次假如二级缓存没有，都要去遍历一遍所有的后置处理器+所有的bean配置

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

![beanå®ä¾åè¿ç¨](md\76.png)

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

![è¿éåå¾çæè¿°](md\36.png)

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

## Spring注解

@RequestParam：用于将请求参数区数据映射到功能处理方法的参数上。

@PathVariable：用于将请求URL中的模板变量映射到功能处理方法的参数上。

@Responsebody：表示该方法的返回结果直接写入HTTP response body中。

@Param：映射器的方法需要多个参数

## SpingMVC

#### MVC框架

![img](md\37.png)

#### 什么是Spring的MVC框架？

Spring 配备构建Web 应用的全功能MVC框架。Spring可以很便捷地和其他MVC框架集成，如Struts，Spring 的MVC框架用控制反转把业务对象和控制逻辑清晰地隔离。它也允许以声明的方式把请求参数和业务对象绑定。

![SpringMVCè¿è¡åç](md\38.png)

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

- 第一部分进行SpringApplication的初始化模块，配置一些基本的环境变量、资源、构造器、监听器；
- 第二部分实现了应用具体的启动方案，包括启动流程的监听模块、加载配置环境模块、及核心的创建上下文环境模块；
- 第三部分是自动化配置模块，该模块作为springboot自动配置核心，在后面的分析中会详细讨论。在下面的启动程序中我们会串联起结构中的主要功能。

![SpringBoot 应用启动流程图](md\39.png)

# Tomcat

#### tomcat架构

（1）Tomcat中只有一个Server，一个Server可以有多个Service，一个Service可以有多个Connector和一个Container； 
（2）Server掌管着整个Tomcat的生死大权； 
（3）Service 是对外提供服务的； 
（4）Connector用于接受请求并将请求封装成Request和Response来具体处理； 
（5）Container用于封装和管理Servlet，以及具体处理request请求；

#### 自定义类加载器的原因

为了在载入类中指定某些规则: 缓存已经载入成功或失败的类;
为了实现类的重载，开启了一个线程来监控类的变化，每隔15秒更新一次。

#### 自定义类加载器

第一个是Tomcat自身所使用的类加载器CommonClassLoader，会加载jre的lib包及tomcat的lib包的类，遵循类加载的双亲委派机制；

第二个是每个Web应用程序用的，每个web应用程序都有自己专用的WebappClassLoader，优先加载/web-inf/lib下的jar中的class文件，这样就隔离了每个web应用程序的影响，但是webappClassLoader可以设置为不遵循类加载的双亲委派机制。

#### WebappClassLoader

WebappClassLoader的设计方案考虑了优化和安全两方面。例如，它会缓存之前已经载入的类来提升性能。此外，它还会缓存加载失败的类的名字，这样，当再次请求加载同一个类时，类载入器就会直接抛出ClassNotFoundException异常，而不会再尝试查找该类了。

#### tomcat优化

**一:Tomcat 内存优化,启动时告诉JVM我要一块大内存(调优内存是最直接的方式)**

**二:Tomcat 线程优化** 

maxThreads="X" 表示最多同时处理X个连接

minSpareThreads="X" 初始化X个连接

maxSpareThreads="X" 表示如果最多可以有X个线程，一旦超过X个,则会关闭不在需要的线程

acceptCount="X" 当同时连接的人数达到maxThreads时,还可以排队,队列大小为X.超过X就不处理

**三:Tomcat IO优化**

中修改protocol实现（APR,BIO,NIO,AIO）

