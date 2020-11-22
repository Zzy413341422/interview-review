# JAVA基础

## 机器码和字节码区别111

![1](md\1.png)

## 面向对象和面向过程的区别

面向过程

性能比面向对象高，开销比较大，

面向对象

易维护、易复用、易扩展，具有面向对象有封装、继承、多态性的特性，可以设计出低耦合的系统，使系统更加灵活、更加易于维护

## Java 语言有哪些特点

1. 面向对象（封装，继承，多态）；

2. 平台无关性（ Java 虚拟机实现平台无关性）；

   封装
   封装把一个对象的属性私有化，同时提供一些想被外界访问的属性的方法

   继承
   通过使用继承我们能够非常方便地复用以前的代码。

   多态
   直白些就是 相同的事物，调用相同的方法，参数也相同时，但表现的行为却不同。
   在Java中有两种形式可以实现多态：继承（多个子类对同一方法的重写）和接口（实现接口并覆盖接口中同一方法）。

## 8种基本数据类型

![3](md\3.png)

Character,    Byte，Short，Long 的缓存池范围默认都是: -128 到 127。

## JAVA反射，动态代理

```java
Class stuClass = Class.forName("fanshe.field.Student");
```
反射就是程序运行期间JVM可以从方法区获取一个类的对象，进而获取字段和方法和注释。依靠此机制，可以动态的创建一个类的对象和调用对象的方法。

缺点是反射的效率很低，而且会破坏封装，通过反射可以访问类的私有方法，不安全。

```java
 		//创建一个实例对象，这个对象是被代理的对象
        Person zhangsan = new Student("张三");
        //创建一个与代理对象相关联的InvocationHandler
        InvocationHandler stuHandler = new StuInvocationHandler<Person>(zhangsan);
        //创建一个代理对象stuProxy来代理zhangsan，代理对象的每个执行方法都会替换执行Invocation中的invoke方法
        Student stuProxy = (Student) Proxy.newProxyInstance(Student.class.getClassLoader(), Student.class.getInterface, stuHandler)；
		//代理执行上交班费的方法
        stuProxy.giveMoney();
```

Java动态代理比静态代理的优势是实现无侵入式的代码扩展，也就是方法的增强；

## 动态代理的原理

 1、根据代理类接口先得到代理类的类全限名、方法列表、异常列表
 2、根据步骤1中的类全限名、方法列表、异常列表、接口列表生成class文件格式的字节流，其中方法的实现会最终调用InvoationHanlder的invoke方法
 3、使用类加载器加载步骤2中的字节流，创建生成动态代理类对象
 4、使用步骤3中创建生成的代理类对象

## 编译

编译分为 前端编译 和 后端编译 两个部分。简单来说，前端编译的任务是：将Java源程序编译为Class文件；后端编译的任务是：由JVM的解释器 解释执行Class文件。

## 反编译

Java语言中的反编译一般指将class文件转换成java文件。

## 泛型

泛型方法：定义泛型方法时，必须在返回值前边加一个<T>，来声明这是一个泛型方法，还得有一个泛型对象参数

泛型类：在类名后加一个<T>

意义和作用有
类型的参数化，就是可以把类型像方法的参数那样传递。这一点意义非凡。
泛型使编译器可以在编译期间对类型进行检查以提高类型安全，减少运行时由于对象类型不匹配引发的异常。

T<? extends B>：只能读，而无法插入（不同子树并不兼容）；

T<? super B> ：只能插入，而无法读（大转小类型无法在编译时候检查安全）；

## Object的九个方法

equal		hashCode		wait		notify		notifyAll		toString		clone		getClass		finalize

## 访问修饰符

![img](md\4.png)

## 为什么java不支持多继承

1.

```
                A foo()    
               / \    
             /     \    
 foo() B     C foo()    
             \     /    
               \ /    
               D  foo()
```

2.使程序设计变得复杂。

## String 和 StringBuffer、StringBuiler

#### 可变性 　

简单的来说：String 类中使用 final 关键字字符数组保存字符串，private　final　char　value[]，且String类的外部不能访问这个成员变量，所以 String 对象是不可变的。而StringBuilder 与 StringBuffer 都继承自 AbstractStringBuilder 类，在 AbstractStringBuilder 中也是使用字符数组保存字符串char[]value 但是没有用 final 关键字修饰，所以这两种对象都是可变的。

#### 线程安全性

String 中的对象是不可变的，也就可以理解为常量，线程安全。AbstractStringBuilder 是 StringBuilder 与 StringBuffer 的公共父类，定义了一些字符串的基本操作，如 expandCapacity、append、insert、indexOf 等公共方法。StringBuffer 对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。StringBuilder 并没有对方法进行加同步锁，所以是非线程安全的。 　　

#### 性能

每次对 String 类型进行改变的时候，都会生成一个新的 String 对象，然后将指针指向新的 String 对象。StringBuffer 每次都会对 StringBuffer 对象本身进行操作，而不是生成新的对象并改变对象引用。相同情况下使用 StirngBuilder 相比使用 StringBuffer 仅能获得 10%~15% 左右的性能提升，但却要冒多线程不安全的风险。

对字符串进行拼接操作，也就是做"+"运算的时候，分2中情况：
i.表达式右边是纯字符串常量，那么存放在常量池里面。
ii.表达式右边如果存在字符串引用，也就是字符串对象的句柄，那么就存放在堆里面

## String不可变的原因

String可变相当于废了缓存池，影响速度。并且也有String中也有可变的Buffer和Builder，没有必要。

## 浅拷贝和深拷贝

1、浅拷贝：对基本数据类型进行值传递，对引用数据类型进行引用传递般的拷贝，此为浅拷贝。

![/clone-qian.png](md\5.jpg)

2、深拷贝：对基本数据类型进行值传递，对引用数据类型，创建一个新的对象，并复制其内容，此为深拷贝。

![/clone-深.png](md\6.jpg)

## 自动装箱与拆箱

装箱：将基本类型用它们对应的引用类型包装起来；
拆箱：将包装类型转换为基本数据类型；

## 值传递和引用传递

值传递是指将值的副本传递给调用的函数，调用的函数可以改变副本的值，但是并不会影响main函数中的原值。 引用传值，传递的是对象的引用，同一个引用指向相同的实体，所以改变引用指向实体的值，可以影响main函数中实体的值。

## 接口和抽象类的区别是什么 

1.所有方法在接口中不能有实现(Java 8 开始接口方法可以有默认实现），抽象类可以有非抽象的方法（不可实现）和非抽象方法（必须实现）

2.接口中的实例变量默认是 final 类型的，而抽象类中则不一定

3.一个类可以实现多个接口，但最多只能实现一个抽象类

4.一个类实现接口的话要实现接口的所有方法，而抽象类实现抽象方法

5.抽象方法可以有public、protected和default这些修饰符 ，接口方法默认修饰符是public。你不可以使用其它修饰符。

## 成员变量与局部变量的区别有那些

1. 成员变量是属于类的，而局部变量是在方法中定义的变量或是方法的参数；成员变量可以被 public,private,static 等修饰符所修饰，而局部变量不能被访问控制修饰符及 static 所修饰；但是，成员变量和局部变量都能被 final 所修饰；
2. 从变量在内存中的存储方式来看，成员变量是对象的一部分，而对象存在于堆内存，局部变量存在于栈内存
3. 从变量在内存中的生存时间上看，成员变量是对象的一部分，它随着对象的创建而存在，而局部变量随着方法的调用而自动消失。
4. 成员变量如果没有被赋初值，则会自动以类型的默认值而赋值（一种情况例外被 final 修饰的成员变量也必须显示地赋值）；而局部变量则不会自动赋值。

## Java 中的异常处理

Java异常类层次结构图

![Javaå¼å¸¸ç±»å±æ¬¡ç»æå¾](md\7.png)

在以下4种特殊情况下，finally块不会被执行：
1.在finally语句块中发生了异常。
2.在前面的代码中用了System.exit()退出程序。
3.程序所在的线程死亡。
4.关闭CPU。

## Io

```java
		File file=new File("abc.txt");
        InputStream fileInputStream = new FileInputStream(file);
        BufferedInputStream bufferedInputStream=new BufferedInputStream(fileInputStream);
        String content=null;
        byte[] buffer=new byte[10240];
        int flag=0;
        FileOutputStream output = new FileOutputStream(file);
        BufferedOutputStream bufferedOutputStream = new BufferedOutputStream(output);
        while ((flag=bufferedInputStream.read(buffer))!=-1){
            bufferedOutputStream.write(buffer);
//            而flush()表示强制将缓冲区中的数据发送出去,不必等到缓冲区满.
            bufferedOutputStream.flush();
        }
```

### 什么是IO流？

它是一种数据的流从源头流到目的地。比如文件拷贝，输入流和输出流都包括了。输入流从文件中读取数据存储到进程(process)中，输出流从进程中读取数据然后写入到目标文件。

### 字节流和字符流的区别。

字节流是以字节为单位写入写出；字符流是以字符为单位写入写出。

### Java中流类的超类主要由那些？

java.io.InputStream；java.io.OutputStream；java.io.Reader；java.io.Writer

### BufferedInputStream优势

如果input用read()方法，每读取一次就要访问一次硬盘，这种读取的方式效率是很低的。
Buffered类初始化时会创建一个较大的byte数组，一次性从底层输入流中读取多个字节来填充byte数组，当程序读取一个或多个字节时，可直接从byte数组中获取，当内存中的byte读取完后，会再次用底层输入流填充缓冲区数组。

## IO多路复用实现

IO多路复用的优势在于，当处理的消耗对比IO几乎可以忽略不计时，可以处理大量的并发IO，而不用消耗太多CPU/内存。

IO多路复用 + 单进（线）程有个额外的好处，就不会有并发编程的各种坑问题，比如在nginx里，redis里，编程实现都会很简单很多。编程中处理并发冲突和一致性，原子性问题真的是很难，极易出错。如果做不到“处理过程相对于IO可以忽略不计”，IO多路复用的并不一定比线程池方案更好。

比如一个web的服务，用jetty 9的NIO connector，后边是spring svc + JDBC连接数据库。spring svc + JDBC连接数据库这两块的处理延迟相对于NIO来说不能忽略，所以并不能指望用jetty 9的NIO connector换了之前的BIO connector的容器，性能能高不少

#### select

创建一个fd_set(bitmap)，放入内核空间中，内核监听多个描述符，将fd_set返回给对应用户，用户遍历fd_set

存在的问题： 
1. 内置数组的形式使得select的接受的最多请求数受限于FD_SetSIZE(bitmap结构)，1024大小； 
2. 每次调用select前都要重新初始化fd_set，将fd_set从用户空间拷贝到内核态
3. 每次调用select后，都需要将fd从内核态拷贝到用户态； 
4. 轮寻排查当文件描述符个数很多时，效率很低； 

#### poll 

通过使用数组替代fd_set，并且数组可宠用，轮寻排查和传入内核的问题未解决。 

#### epoll 

使用红黑树代替fdset，有数据后重排结构。

红黑树在用户和内核的共享空间上。

## JAVA  NIO

JAVA NIO 包含下面几个核心的组件：

- Channel(通道)：通道是双向的，可读也可写
- Buffer(缓冲区)：即ByteBuffer，在读取数据时，它是直接读到缓冲区中的; 在写入数据时，写入到缓冲区中。
- Selector(选择器)：选择器用于使用单个线程处理多个通道。

## Iterator和ListIterator的区别是什么？

- Iterator可用来遍历Set和List集合，但是ListIterator只能用来遍历List。
- Iterator对集合只能是前向遍历，ListIterator既可以前向也可以后向。
- ListIterator实现了Iterator接口，并包含其他的功能，比如：增加元素，替换元素，获取前一个和后一个元素的索引，等等。

# JAVA CONCURRENT

## 同步、异步与阻塞、非阻塞的理解

同步和异步关注的是**消息通信机制**。所谓同步，就是在发出一个*调用*时，在没有得到结果之前，该*调用*就不返回。但是一旦调用返回，就得到返回值了。换句话说，就是由*调用者*主动等待这个*调用*的结果。

而异步则是相反，**调用在发出之后，这个调用就直接返回了，所以没有返回结果**。换句话说，当一个异步过程调用发出后，调用者不会立刻得到结果。而是在*调用*发出后，*被调用者*通过状态、通知来通知调用者，或通过回调函数处理这个调用。

阻塞和非阻塞关注的是**程序在等待调用结果（消息，返回值）时的状态.**

阻塞调用是指调用结果返回之前，当前线程会被挂起。调用线程只有在得到结果之后才会返回。
## Synchronized:

#### JVM实现底层

加锁机制成功标志：CAS

线程阻塞机制（futex_wait）：在将进程阻塞前会将当期进程插入到一个等待队列中，需要注意的是这里说的等待队列其实是一个类似Java HashMap的结构，全局唯一。

线程唤醒机制（futex_wake）：遍历fb的链表，找到uaddr对应的节点，调用wake_futex唤起等待的进程。



![](C:\Users\jimmiezeng\IdeaProjects\interview-review\md\100.png)

当一个线程尝试获得锁时，如果该锁已经被占用，则会将该线程封装成一个ObjectWaiter对象插入到cxq的队列的队首，然后调用park函数挂起当前线程。在linux系统上，park函数底层调用的是gclib库的pthread_cond_wait，JDK的ReentrantLock底层也是用该方法挂起线程的。

当线程释放锁时，会从cxq或EntryList中挑选一个线程唤醒，被选中的线程叫做Heir presumptive即假定继承人（应该是这样翻译），就是图中的Ready Thread，假定继承人被唤醒后会尝试获得锁，但synchronized是非公平的，所以假定继承人不一定能获得锁（这也是它叫"假定"继承人的原因）。

如果线程获得锁后调用Object#wait方法，则会将线程加入到WaitSet中，当被Object#notify唤醒后，会将线程从WaitSet移动到cxq或EntryList中去。需要注意的是，当调用一个锁对象的wait或notify方法时，如当前锁的状态是偏向锁或轻量级锁则会先膨胀成重量级锁。

![img](md\86.jpg)



## 各种锁

#### 悲观锁

每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会阻塞直到它拿到锁

#### 乐观锁

每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号机制和CAS算法实现。

#### 自旋锁

自旋锁是指尝试获取锁的线程不会立即阻塞，而是采用循环的方式去尝试获取锁，这样的好处是减少线程上下文切换的消耗，缺点是循环会消耗CPU。

#### 偏向锁

偏向锁是指一段同步代码一直被一个线程所访问，那么该线程会自动获取锁。降低获取锁的代价。

只需要在Mark Word中CAS记录owner（本质上也是更新，但初始值为空），如果记录成功，则偏向锁获取成功，记录锁状态为偏向锁，以后当前线程等于owner就可以零成本的直接获得锁；否则，说明有其他线程竞争，膨胀为轻量级锁。

#### 轻量级锁和重量级锁

轻量级锁是指当锁是偏向锁的时候，不存在竞争前提下，被另一个线程所访问，偏向锁就会升级为轻量级锁，其他线程会通过自旋的形式尝试获取锁，不会阻塞，提高性能。

使用轻量级锁时，不需要申请互斥量，仅仅_将Mark Word中的部分字节CAS更新指向线程栈中的Lock Record，如果更新成功，则轻量级锁获取成功_，记录锁状态为轻量级锁；否则，说明已经有线程获得了轻量级锁，目前发生了锁竞争（不适合继续使用轻量级锁），接下来膨胀为重量级锁。



重量级锁是指当锁为轻量级锁的时候，另一个线程虽然是自旋，但自旋不会一直持续下去，当自旋一定次数的时候，还没有获取到锁，就会进jianb入阻塞，该锁膨胀为重量级锁。重量级锁会让其他申请的线程进入阻塞，性能降低。

#### 锁消除

(有锁但不存在竞争，锁多余)：JVM编译优化，将不存在数据竞争的锁消除

#### 锁粗化

![img](md\9.jpg)

类似上图中的append方法，如果虚拟机探测到有一串零碎操作都是对同一对象加锁，将会把加锁同步的范围扩展到整个操作序列的外部，也就是在第一个和最后一个append操作之后。

## 乐观锁实现

##### 版本号

update player set coins = {0}, version = version + 1 where player_id = {1} and version = {2}

##### CAS

CAS操作有三个操作数V（内存地址），A（旧的预期值），B（准备设置的新值）。指令执行时先看V指向的内存中存储的值是否和A相同，如果相同才会更新为B，否则什么也不做。

在多个线程同时CAS的情况下是不会发生多个线程CAS成功的情况的，因为计算机底层实现保证了V指向内存的互斥性和立即可见性，可以理解为 CAS操作是底层保证的线程安全

缺点：CAS：ABA问题、CAS自旋消耗CPU、只能保证一个共享变量的原子操作。

## 谈谈 synchronized和ReenTrantLock 的区别

![](md\92.png)

## Volatile:

#### Java内存模型

![img](md\10.png)

内存屏障（[memory barrier](http://en.wikipedia.org/wiki/Memory_barrier)）是一个CPU指令。基本上，它是这样一条指令： a) 确保一些特定操作执行的顺序； b) 影响一些数据的可见性(可能是某些指令执行后的结果)。编译器和CPU可以在保证输出结果一样的情况下对指令重排序，使性能得到优化。插入一个内存屏障，相当于告诉CPU和编译器先于这个命令的必须先执行，后于这个命令的必须后执行。内存屏障另一个作用是强制更新一次不同CPU的缓存。例如，一个写屏障会把这个屏障前写入的数据刷新到缓存，这样任何试图读取该数据的线程将得到最新值，而不用考虑到底是被哪个cpu核心或者哪颗CPU执行的。

内存屏障（[memory barrier](http://en.wikipedia.org/wiki/Memory_barrier)）和volatile什么关系？上面的虚拟机指令里面有提到，如果你的字段是volatile，Java内存模型将在写操作后插入一个写屏障指令，在读操作前插入一个读屏障指令。这意味着如果你对一个volatile字段进行写操作，你必须知道：1、一旦你完成写入，任何访问这个字段的线程将会得到最新的值。2、在你写入前，会保证所有之前发生的事已经发生，并且任何更新过的数据值也是可见的，因为内存屏障会把之前的写入值都刷新到缓存。

当进行IO操作或者线程内部调用`synchronized`修饰的方法或者同步代码块时，线程的缓存会进行刷新

#### 可见性

```
线程1： load i from 主存    // i = 0
        i + 1  // i = 1
线程2： load i from主存  // 因为线程1还没将i的值写回主存，所以i还是0
        i +  1 //i = 1
线程1:  save i to 主存
线程2： save i to 主存
```

#### 指令重排序

```
int a = 0;
bool flag = false;
public void write() {
    a = 2;              //1
    flag = true;        //2
}
public void multiply() {
    while (flag) {         //3
        int ret = a * a;//4
        flag=false;
    }
}
```

#### volatile作用

多线程编程时候的状态标志，否则修改后看不到，只会从线程缓存中取值

#### 说说 synchronized 关键字和 volatile 关键字的区别

synchronized关键字和volatile关键字比较
1.volatile关键字是线程同步的轻量级实现，所以volatile性能肯定比synchronized关键字要好。
2.但是volatile关键字只能用于变量而synchronized关键字可以修饰方法以及代码块。
3.多线程访问volatile关键字不会发生阻塞，而synchronized关键字可能会发生阻塞
4.volatile不能保证数据的原子性。synchronized关键字能保证。

volatile的一个重要作用就是和CAS结合，保证了原子性

## 线程池：

#### 为什么要用线程池？

（1）降低资源消耗。 
（2）提高响应速度。 当任务到达时，任务可以不需要的等到线程创建就能立即执行。
（3）提高线程的可管理性。 线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源，还会降低系统的稳定性，使用线程池可以进行统一的分配，调优和监控。

#### 常见的线程池：

①SingleThreadExecutor
核心线程数=最大线程数=1；
②FixedThreadExecutor(n)
核心线程数=最大线程数
③CacheThreadExecutor（推荐使用）
核心线程数=0，最大线程数=Integer.MAX_VALUE
④ScheduleThreadExecutor
定时任务线程池

ThreadPoolExecutor:

corePoolSize - 池中所保存的线程数，包括空闲线程。

maximumPoolSize-池中允许的最大线程数。

keepAliveTime - 当线程数大于核心时，此为终止前多余的空闲线程等待新任务的最长时间。

unit  keepAliveTime参数的时间单位

BlockingQueue<Runnable>：阻塞队列

threadFactory 执行者创建新线程时使用的工厂

RejectedExecutionHandler类型的变量，表示线程池的饱和策略。

线程池提供了4种策略：
1.AbortPolicy：直接抛出异常，这是默认策略；

2.CallerRunsPolicy：用调用者所在的线程来执行任务；

3.DiscardOldestPolicy：丢弃阻塞队列中靠最前的任务，并执行当前任务；

4.DiscardPolicy：直接丢弃任务；

#### 线程池执行过程：

 ![è¿éåå¾çæè¿°](https://img-blog.csdn.net/20170618213838961?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTI0MDg3Nw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### 线程池调优:

1）设置动态线程池。

2）压测。

##### CPU密集型任务(CPU个数个线程+1)

计算密集型，顾名思义就是应用需要非常多的CPU计算资源，在多核CPU时代，我们要让每一个CPU核心都参与计算，将CPU的性能充分利用起来

##### IO密集型任务(CPU个数两倍的线程)

于IO密集型的应用，就很好理解了，我们现在做的开发大部分都是WEB应用，涉及到大量的网络传输，不仅如此，与数据库，与缓存间的交互也涉及到IO，一旦发生IO，线程就会处于等待状态，当IO结束，数据准备好后，线程才会继续执行。因此从这里可以发现，对于IO密集型的应用，我们可以多设置一些线程池中线程的数量，这样就能让在等待IO的这段时间内，线程可以去做其它事，提高并发处理效率。

#### 实现Runnable接口和Callable接口的区别

如果想让线程池执行任务的话需要实现的Runnable接口或Callable接口。 Runnable接口或Callable接口实现类都可以被ThreadPoolExecutor或ScheduledThreadPoolExecutor执行。两者的区别在于 Runnable 接口不会返回结果但是 Callable 接口可以返回结果。

#### 什么是FutureTask和Future?

Future表示一个任务的生命周期，并提供了相应的方法来判断是否已经完成或取消，以及获取任务的结果和取消任务等

#### FutureTask是Future的实现

```java
class FutureTask<V> implements RunnableFuture<V>
public interface RunnableFuture<V> extends Runnable, Future<V>
1.作运算结果
FutureTask <Integer> result2 = mExecutor.submit(new Callable<Integer>() {  
                @Override  
                public Integer call() throws Exception {              
   }
            });  
2.作任务的封装提交，直接得到结果
 FutureTask<Integer> futureTask = new FutureTask<Integer>(  
                    new Callable<Integer>() {  
                        @Override  
                        public Integer call() throws Exception {  
                            return fibc(20);  
                        }  
                    });  
            // 提交futureTask  
            mExecutor.submit(futureTask) ;

```

#### 执行execute()方法和submit()方法的区别是什么呢？

1)execute() 方法用于提交不需要返回值的任务，所以无法判断任务是否被线程池执行成功与否；
2)submit()方法用于提交需要返回值的任务。线程池会返回一个future类型的对象，通过这个future对象可以判断任务是否执行成功，并且可以通过future的get()方法来获取返回值，get()方法会阻塞当前线程直到任务完成，而使用 get（long timeout，TimeUnit unit）方法则会阻塞当前线程一段时间后立即返回，这时候有可能任务没有执行完。

#### 线程池复用原理

```
while (task != null || (task = getTask()) != null) {}
getTask（）{
	Runnable r = timed ?
                workQueue.poll(keepAliveTime, TimeUnit.NANOSECONDS) :
                workQueue.take();//阻塞队列
}
```

#### 多个核心线程去take阻塞队列中线程，谁能拿到了？

阻塞队列由ReentrantLock和Condition实现，因此可实现阻塞的公平锁和非公平锁。

## Atom:

#### AtomicInteger 线程安全原理简单分析

AtomicInteger 类的部分源码：
AtomicInteger 类主要利用 CAS (compare and swap) + volatile 和 native 方法来保证原子操作，从而避免 synchronized 的高开销，执行效率大为提升。

#### CAS的原理:

CAS机制当中使用了3个基本操作数：内存地址V，旧的预期值A，要修改的新值B。

更新一个变量的时候，只有当变量的预期值A和内存地址V当中的实际值相同时，才会将内存地址V对应的值修改为B。

## AQS :

#### AQS核心思想

AQS是一种提供了原子式管理同步状态、阻塞和唤醒线程功能以及队列模型的简单框架。如果被请求的共享资源空闲，则将当前请求资源的线程设置为有效的工作线程，并且将共享资源设置为锁定状态。如果被请求的共享资源被占用，那么就需要一套线程阻塞等待以及被唤醒时锁分配的机制，这个机制AQS是用CLH队列锁实现的，即将暂时获取不到锁的线程加入到队列中。 

![AQSåçå¾](md\13.png)

#### AQS阻塞等待以及被唤醒时锁分配的机制

新加入的节点不断自旋如下操作

![](md\94.png)

判断↑是否需要阻塞当前线程

![](md\95.png)

#### AQS定义两种资源共享方式

Exclusive（独占）：只有一个线程能执行，如ReentrantLock。又可分为公平锁和非公平锁：
公平锁：按照线程在队列中的排队顺序，先到者先拿到锁
非公平锁：当线程要获取锁时，无视队列顺序直接去抢锁，谁抢到就是谁的
Share（共享）：多个线程可同时执行，如Semaphore/CountDownLatch、Semaphore、CountDownLatCh、 CyclicBarrier、ReadWriteLock 我们都会在后面讲到。

#### AQS 组件总结

Semaphore(信号量)-允许多个线程同时访问： synchronized 和 ReentrantLock 都是一次只允许一个线程访问某个资源，Semaphore(信号量)可以指定多个线程同时访问某个资源。
CountDownLatch （倒计时器）： CountDownLatch是一个同步工具类，它允许一个或多个线程一直等待，直到其他线程的将CountDownLatch减到零后再执行。
CyclicBarrier(循环栅栏)： 让一组线程到达一个屏障时被阻塞，直到最后一个线程到达屏障时，屏障才会开门，所有被屏障拦截的线程才会继续干活。

## 线程

![img](md\14.jpg)

#### 实现线程的3种方法：

继承Thread类创建线程
实现Runnable接口创建线程
实现Callable接口
线程池

#### Java中如何停止一个线程？

thread. Interrupt()

if(this.interrupted()) {do end};

#### 说说wait,yield,sleep,join?

sleep() : 它可以让当前正在执行的线程在指定的时间内暂停执行，进入阻塞状态
wait():与sleep相同，但wait会释放锁；
yield()：yield()方法和sleep()方法类似，也不会释放“锁标志”，区别在于，它没有参数，即yield()方法只是使当前线程重新回到可执行状态
join()： join()方法会使当前线程等待调用join()方法的线程结束后才能继续执行

#### 为什么wait, notify 和 notifyAll这些方法不在thread类里面？

一个很明显的原因是JAVA提供的锁是对象级的而不是线程级的，每个对象都有锁，通过线程获得。如果线程需要等待某些锁那么调用对象中的wait()方法就有意义了。如果wait()方法定义在Thread类中，线程正在等待的是哪个锁就不明显了。简单的说，由于wait，notify和notifyAll都是锁级别的操作，所以把他们定义在Object类中因为锁属于对象。

#### 为什么wait和notify方法要在同步块中调用？

主要是因为Java API强制要求这样做，如果你不这么做，你的代码会抛出IllegalMonitorStateException异常。还有一个原因是为了避免wait和notify之间产生竞态条件。

#### 怎么检测一个线程是否拥有锁？

在java.lang.Thread中有一个方法叫holdsLock()，它返回true如果当且仅当当前线程拥有某个具体对象的锁。

#### 如果同步块内的线程抛出异常会发生什么？

无论同步块是正常还是异常退出的，里面的线程都会释放锁。

## 阻塞队列

#### 什么是阻塞队列？

java.util.concurrent.BlockingQueue的特性是：当队列是空的时，从队列中获取或删除元素的操作将会被阻塞，或者当队列是满时，往队列里添加元素的操作会被阻塞。特别地，阻塞队列不接受空值，当你尝试向队列中添加空值的时候，它会抛出NullPointerException。另外，阻塞队列的实现都是线程安全的，所有的查询方法都是原子的并且使用了内部锁或者其他形式的并发控制。

#### LinkedBlockingQueue、ArrayBlockingQueue、DelayQueue、SynchronousQueue

1. SynchronousQueue是无界的，是一种无缓冲的等待队列
2. LinkedBlockingQueue是无界的，是一个无界缓存的等待队列。
3. ArrayListBlockingQueue是有界的，是一个有界缓存的等待队列。
4. DelayQueue是一个支持延时获取元素的无界阻塞队列

#### 几种方法：

remove   移除并返回队列头部的元素，没有则抛出一个异常
poll          移除并返问队列头部的元素，没有则返回null
take         移除并返回队列头部的元素，没有则自旋

add          增加一个元索，满则抛出一个异常
offer        添加一个元素，如果队列已满，则返回false
put           添加一个元素，如果队列满，则自旋

element  返回队列头部的元素，如果队列为空，则抛出一个异常
peek        返回队列头部的元素，如果队列为空，则返回null

## ThreadLocal及其引发的内存泄露

![ThreadLocalæ°æ®è¯»ååè®¾ç½®è¿ç¨](md\15.jpg)

当释放掉对threadlocal对象的强引用后，map里面的value没有被回收，但却永远不会被访问到了，因此ThreadLocal存在着内存泄露问题。在不使用该ThreadLocal对象时，及时调用该对象的remove方法去移除ThreadLocal.ThreadLocalMap中的对应Entry。

ThreadLocalMap 中解决 Hash 冲突的方式并非链表的方式，而是采用线性探测的方式。还有一种再散列方法；

应用：链路追踪、Spring的事务传播

# 集合类

![img](md\16.jpg)

## HashMap

#### 底层数据结构分析

JDK1.8 之前 HashMap 底层是 数组和链表 结合在一起使用也就是 链表散列。

HashMap 通过 key 的 hashCode 经过用高16位异或低16位处理过后得到 hash 值，然后通过 (n - 1) & hash 判断当前元素存放的位置

(n-1)&hash=hash%n，且速度更快；

使用扰动函数之后可以减少碰撞。

#### loadFactor加载因子

loadFactor太大导致查找元素效率低，太小导致数组的利用率低

#### put方法

![putæ¹æ³](md\17.png)

#### HashMap的线程不安全主要体现在下面两个方面：

1.在JDK1.7中，当并发执行扩容操作时会造成环形链和执行put操作数据丢失的情况。

```
for (Entry<K,V> e : table) {
            while(null != e) {
                Entry<K,V> next = e.next;//两条线程同时执行到这里
                int i = indexFor(e.hash, newCapacity);
                e.next = newTable[i];
                newTable[i] = e;
                e = next;
            }
}
```

2.在JDK1.8中，在并发执行put操作时会发生数据覆盖的情况。（通过尾插+分两条链表后赋值的方法解决成环问题，若只通过尾插仍有成环问题）。 

#### HashMap 扩容原理

resize 就是自动扩容，当当前数据存储的数量（即size()）大小必须大于等于阈值；并且当前加入的数据是否发生了hash冲突。以后会扩容到原来的 2 倍，关键代码 newCap = oldCap << 1。但是这里有一个非常巧妙的解决方法，因为扩容是扩充的 2 倍，n-1 转换为二进制也就是高位变成了1，那么根据(n - 1) & hash 计算，如果 hash 高位是 1 那么新的 index 位置就是 oldIndex + 16，如果hash 的高位 是 0 ，那么 index 的位置就是原来的 oldIndex 的位置，这样直接判断高位就可以了，省去了重新计算hash。

#### HashMap什么时候扩容

1.8扩容会发生在两种情况下（满足任意一种条件即发生扩容）：
　　　　　　a 当前存入数据数量大于阈值即发生扩容
　　　　　　b 存入数据到某一条链表上，此时数据大于8，且数组长度小于64即发生扩容
1.7 a存放新值的时候当前已有元素的个数必须大于等于阈值
b存放新值的时候当前存放数据发生hash碰撞（当前key计算的hash值换算出来的数组下标位置已经存在值）

## HashMap 和 Hashtable 的区别

线程是否安全： HashMap 是非线程安全的，HashTable 是线程安全的；HashTable 内部的方法基本都经过 synchronized 修饰。
效率： 因为线程安全的问题，HashMap 要比 HashTable 效率高一点。另外，HashTable 基本被淘汰，不要在代码中使用它；
对Null key 和Null value的支持： HashMap 中，null 可以作为键，这样的键只有一个，可以有一个或多个键所对应的值为 null。。但是在 HashTable 中 put 进的键值只要有一个 null，直接抛出 NullPointerException。
初始容量大小和每次扩充容量大小的不同 ： ①创建时如果不指定容量初始值，Hashtable 默认的初始大小为11，之后每次扩充，容量变为原来的2n+1。HashMap 默认的初始化大小为16。之后每次扩充，容量变为原来的2倍。②创建时如果给定了容量初始值，那么 Hashtable 会直接使用你给定的大小，而 HashMap 会将其扩充为2的幂次方大小
底层数据结构： JDK1.8 以后的 HashMap 在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为8）时，将链表转化为红黑树，以减少搜索时间。Hashtable 没有这样的机制。

## HashMap 的长度为什么是2的幂次方

length-1 二进制中为1的位数越多，那么分布就平均

hash%length==hash&(length-1)的前提是 length 是2的 n 次方

## HashMap1.7和1.8区别

1.7中采用数组+链表，1.8采用的是数组+链表/红黑树，即在1.7中链表长度超过一定长度后就改成红黑树存储。
1.7扩容时需要重新计算哈希值和索引位置，1.8并不重新计算哈希值，巧妙地采用和扩容后容量进行&操作来计算新的索引位置。
1.7是采用表头插入法插入链表，1.8采用的是尾部插入法。

## HashSet 和 HashMap 区别

HashSet底层是Hashmap；把放入的对象作为key放入hashmap中。所以没有重复的对象。

## ConcurrentHashMap 和 Hashtable和SychronizedMap

JDK1.7
首先将数据分为一段一段的存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据时，其他段的数据也能被其他线程访问。

#### ConcurrentHashMap 

| 执行put的线程                                        | 执行get的线程                  |
| ---------------------------------------------------- | ------------------------------ |
| ⑧tab[index] = new HashEntry(key, hash, first, value) |                                |
|                                                      | ③if (count != 0)               |
| ②count + 1 = 1                                       |                                |
|                                                      | ⑨HashEntry e = getFirst(hash); |

弱一致性，synchronized只在写的时候锁定当前链表或红黑二叉树的首节点，其他的就和hashmap和hashtable的区别一样

#### put方法

若头结点无值，则通过cas插入。

若头结点有值，则通过synchronize插入。

若头结点处于扩容状态，则协助扩容。比如有个数组长度64的，然后扩容的时候变成长度128，我们把扩容操作当做一个队列来看，每个线程来帮助扩容，其实就是去队列领取对应的长度。那么假设现在有4个线程来访问，第一个线程访问，看到要扩容，则领取队列中领取数组的0-15的数组数据，这里我们有个标志位a(从后往前减)，a=64-16=47,线程2过来，发现这个标志位为正在扩容，则去领取16-31的数组长度进行扩容 a=31,线程3来，一样，领取32-47的数组长度a=15，线程4来了，领取47-63的数组长度a=0。这就是帮助扩容。然后线程5来了，看到a=0,则这个时候，这个标志a其实已经移动到队列末尾了，也就没有要扩容的数据，所以就不用帮助扩容。大概意思就是这样。

#### Hashtable

方法上加锁

#### SychronizedMap

对象锁，方法都要获取一个变量mutex的锁

## LinkedHashmap实现Lru

```java
public class LRUCache {
    private int capacity;
    private Map<Integer, Integer> cache;
    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.cache = new LinkedHashMap<Integer, Integer> (capacity, 0.75f, true) {
            // 定义put后的移除规则，大于容量就删除eldest
            protected boolean removeEldestEntry(Map.Entry<Integer, Integer> eldest) {
                return size() > capacity;
            }
        };
    }
 }
```

## ArrayLisy

它继承于 **AbstractList**，实现了 **List**, **RandomAccess**, **Cloneable**, **java.io.Serializable** 这些接口。

以无参数构造方法创建 ArrayList 时，实际上初始化赋值的是一个空数组。当真正对数组进行添加元素操作时，才真正分配容量。即向数组中添加第一个元素时，数组容量扩为10。然后每次扩容为1.5倍

在JDK1.7中，如果通过无参构造的话，初始数组容量为0，当真正对数组进行添加时，才真正分配容量。
每次按照1.5倍（位运算）的比率通过copeOf的方式扩容。 
在JKD1.6中，如果通过无参构造的话，初始数组容量为10.每次通过copeOf的方式扩容后容量为原来的1.5倍加1.以上就是动态扩容的原理。

数据量大的优化措施：向 ArrayList 添加大量元素之前最好先使用`ensureCapacity` 方法，以减少增量重新分配的次数

## CopyOnWriteArrayList

CopyOnWriteArrayList 类的所有可变操作（add，set等等）都是通过创建底层数组的新副本来实现的。当 List 需要被修改的时候，我并不修改原有内容，而是对原有数据进行一次复制，将修改的内容写入副本。写完之后，再将修改完的副本替换原来的数据，这样就可以保证写操作不会影响读操作了。

## LinkedList

LinkedList 底层使用的是双向链表数据结构（JDK1.6之前为循环链表，JDK1.7取消了循环。）

## Arraylist 与 LinkedList 异同

1. 是否保证线程安全： ArrayList 和 LinkedList 都是不同步的，也就是不保证线程安全；

2. 底层数据结构： Arraylist 是Object数组；LinkedList 是双向链表;

3. 理论上：往表中加入大量数据LinkedList更优，删增删指定位置LinkedList更优，查指定位置ArrayList

   实际上：增删查指定位置ArrayList遥遥领先，往表中加入大量数据ArrayList稍优

4. 是否支持快速随机访问： LinkedList 不支持高效的随机元素访问，而 ArrayList 支持。快速随机访问就是通过元素的序号快速获取元素对象(对应于get(int index) 方法)。

5. 内存空间占用： ArrayList的空间浪费主要体现在在list列表的结尾会预留一定的容量空间，而LinkedList的空间花费则体现在它的每一个元素都需要消耗比ArrayList更多的空间（因为要存放直接后继和直接前驱以及数据）。

   ####  list 的遍历方式选择：

   实现了RandomAccess接口的list，优先选择普通for循环 
   未实现RandomAccess接口的list，foreach遍历

# 设计模式

## 创建型模式

#### 单例模式：**保证类只有一个实例，并提供一个访问他的全局访问点**

懒汉式单例：

```java
public class Singleton4 {
    // 私有构造
    private Singleton4() {}
    private volatile  static Singleton4 single = null;
    // 双重检查
    public  static Singleton4 getInstance() {
        if (single == null) {
            synchronized (Singleton4.class) {
                if (single == null) {
                    single = new Singleton4();
                }
            }
        }
        return single;
    }
}
```

```
public class Singleton {
    private Singleton(){
    }
    public static Singleton getInstance(){
        return SingletonHolder.INSTANCE;
    }
    private static class SingletonHolder {
        private static final Singleton INSTANCE = new Singleton();
    }
}
```

```
public enum Singleton {
    INSTANCE;
    public void doSth(){
        //do stuff
    }
}
```
#### **简单工厂模式** ： 用来生产同一等级结构中的任意产品。

缺点：对于增加新的产品，无能为力

一个没有实现接口的工厂类可以生产任何东西。

#### 工厂方法模式 ：用来生产同一等级结构中的固定产品。

优点：支持增加任意产品

多个工厂实现一个接口，生产多种产品

#### 抽象工厂模式 ：用来生产不同产品族的全部产品。

缺点：不能增加新的产品；优点：支持增加产品族  

多个工厂继承一个抽象工厂类

## 结构性模式

装饰者模式：**重点在于功能的扩展，添加额外的职责**.

适配器模式：**使得原本不兼容，不能够一起工作的那些类能够一起工作**

#### 装饰者模式：

```Java
public class Decorator implements Component{
        private Component component;
        public Decorator(Component component){
            this.component = component
        }
       public void operation(){
            component.operation();
       }
}

```

#### 适配器模式：

```java
// Target 目标接口
public interface Volt5 {
    public int getVolt5();
}

// Adaptee 需要被转换的对象
public class Volt220 {
    public int getVolt220() {
        return 220;
    }
}

//Adapter 适配器 
public class VoltAdapter implements Volt5 {

    Volt220 mVolt220;

    public VoltAdapter(Volt220 adaptee) {
        mVolt220 = adaptee;
    }

    public int getVolt220() {
        return 220;
    }

    @Override
    public int getVolt5() {
        return 5;
    }
}
```

#### 观察者模式：

发布订阅模式

```java
public class Subject {
   
   private List<Observer> observers 
      = new ArrayList<Observer>();
   private int state;
 
   public int getState() {
      return state;
   }
 
   public void setState(int state) {
      this.state = state;
      notifyAllObservers();
   }
 
   public void attach(Observer observer){
      observers.add(observer);      
   }
 
   public void notifyAllObservers(){
      for (Observer observer : observers) {
         observer.update();
      }
   }  
}

class BinaryObserver extends Observer{
 
   public BinaryObserver(Subject subject){
      this.subject = subject;
      this.subject.attach(this);
   }
 
   @Override
   public void update() {
      System.out.println( "Binary String: " 
      + Integer.toBinaryString( subject.getState() ) ); 
   }
}
```

#### Spring中使用到的设计模式

https://blog.csdn.net/caoxiaohong1005/article/details/80039656