





# JAVA基础

## 机器码和字节码区别

![Javaç¨åºè¿è¡è¿ç¨](https://camo.githubusercontent.com/8f6eceddf64b5948c69a398d1a0e777c9c7f8e5b/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f4a6176612532302545372541382538422545352542412538462545382542462539302545382541312538432545382542462538372545372541382538422e706e67)

## 面向对象和面向过程的区别

面向过程

性能比面向对象高，开销比较大，

面向对象

易维护、易复用、易扩展，具有面向对象有封装、继承、多态性的特性，可以设计出低耦合的系统，使系统更加灵活、更加易于维护

## Java 语言有哪些特点

1. 面向对象（封装，继承，多态）；
2. 平台无关性（ Java 虚拟机实现平台无关性）；

## 什么是 JDK 什么是 JRE 什么是 JVM 三者之间的联系与区别

1.JDK 用于开发，JRE 用于运行java程序 ；
2.JDK 和 JRE 中都包含 JVM ；
3.JVM 是 java 编程语言的核心并且具有平台独立性。

## Java和C++的区别

•	都是面向对象的语言，都支持封装、继承和多态
•	Java 不提供指针来直接访问内存，程序内存更加安全
•	Java 的类是单继承的，C++ 支持多重继承；虽然 Java 的类不可以多继承，但是接口可以多继承。
•	Java 有自动内存管理机制，不需要程序员手动释放无用内存

## 8种基本数据类型

![img](https://camo.githubusercontent.com/d913ab9b3880feab7d326a0904caac5f5e285a56/687474703a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f31382d392d31352f38363733353531392e6a7067)

Character,    Byte，Short，Long 的缓存池范围默认都是: -128 到 127。

## JAVA中为什么需要代理？

1.方便远程调用类

2.增强方法，面向切面编程思想

## JAVA反射，动态代理

```java
Class stuClass = Class.forName("fanshe.field.Student");
```

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

## 泛型

泛型方法：定义泛型方法时，必须在返回值前边加一个<T>，来声明这是一个泛型方法，还得有一个泛型对象参数

泛型类

## 访问修饰符

![img](https://images2015.cnblogs.com/blog/690292/201609/690292-20160923095944481-1758567758.png)

## 重载和重写的区别

重载： 发生在同一个类中，方法名必须相同，参数必须不同，方法返回值和访问修饰符可以不同，发生在编译时。 　　
重写： 发生在父子类中，方法名、参数列表必须相同，返回值范围小于等于父类，抛出的异常范围小于等于父类，访问修饰符范围大于等于父类；如果父类方法访问修饰符为 private 则子类就不能重写该方法。

## Java 面向对象编程三大特性:封装、继承、多态

封装

封装把一个对象的属性私有化，同时提供一些想被外界访问的属性的方法

继承
通过使用继承我们能够非常方便地复用以前的代码。
多态
指一个引用（类型）在不同的情况下的多种状态
在Java中有两种形式可以实现多态：继承（多个子类对同一方法的重写）和接口（实现接口并覆盖接口中同一方法）。

## String 和 StringBuffer、StringBuiler封装

#### 可变性 　

简单的来说：String 类中使用 final 关键字字符数组保存字符串，private　final　char　value[]，所以 String 对象是不可变的。而StringBuilder 与 StringBuffer 都继承自 AbstractStringBuilder 类，在 AbstractStringBuilder 中也是使用字符数组保存字符串char[]value 但是没有用 final 关键字修饰，所以这两种对象都是可变的。

#### 线程安全性

String 中的对象是不可变的，也就可以理解为常量，线程安全。AbstractStringBuilder 是 StringBuilder 与 StringBuffer 的公共父类，定义了一些字符串的基本操作，如 expandCapacity、append、insert、indexOf 等公共方法。StringBuffer 对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。StringBuilder 并没有对方法进行加同步锁，所以是非线程安全的。 　　

#### 性能

每次对 String 类型进行改变的时候，都会生成一个新的 String 对象，然后将指针指向新的 String 对象。StringBuffer 每次都会对 StringBuffer 对象本身进行操作，而不是生成新的对象并改变对象引用。相同情况下使用 StirngBuilder 相比使用 StringBuffer 仅能获得 10%~15% 左右的性能提升，但却要冒多线程不安全的风险。

对字符串进行拼接操作，也就是做"+"运算的时候，分2中情况：

i.表达式右边是纯字符串常量，那么存放在栈里面。

ii.表达式右边如果存在字符串引用，也就是字符串对象的句柄，那么就存放在堆里面。

## 自动装箱与拆箱

装箱：将基本类型用它们对应的引用类型包装起来；
拆箱：将包装类型转换为基本数据类型；

## 值传递和引用传递

值传递和引用传递，属于方法调用时参数的求值策略。

## 枚举

枚举就是在一个类里定义几个常量，每个常量都是这个类的实例，用于替代常量类。

你可以像使用普通 Java 类一样来使用枚举类型。所有的*枚举*都*继承*自*java*.lang.*Enum*类。可以实现接口，写方法；

```java
public enum  ResultEnum {
    CLASS_ZERO(0,"此课程已被人选完");
    private Integer code;
    private String message;
    ResultEnum(Integer code, String message) {
        this.code = code;
        this.message = message;
    }
}
```



## 内部类，静态内部类

一个.java文件中可以有多个同级类，该文件同级的类之间可以互相调用，但是除了public的类，其他不能够在其他文件调用。

```
public class Test{
    public static void main(String[] args){
           // 初始化Bean
           Test test = new Test();    
			Test.Bean1 bean1 = test.new Bean1(); 
           bean1.I++;
           // 初始化Bean2
           Test.Bean2 b2 = new Test.Bean2();
           bean2.J++;
           //初始化Bean3
           Bean bean = new Bean();    
			Bean.Bean3 bean3 =  bean.new Bean3();
           bean3.k++;
    }
    class Bean1{
           public int I = 0;
    }
    static class Bean2{
           public int J = 0;
    }
}
class Bean{
    class Bean3{
           public int k = 0;
    }
}
```

## 接口和抽象类的区别是什么 

1.所有方法在接口中不能有实现(Java 8 开始接口方法可以有默认实现），抽象类可以有非抽象的方法

2.接口中的实例变量默认是 final 类型的，而抽象类中则不一定

3.一个类可以实现多个接口，但最多只能实现一个抽象类

4.一个类实现接口的话要实现接口的所有方法，而抽象类不一定

## 成员变量与局部变量的区别有那些

1. 成员变量是属于类的，而局部变量是在方法中定义的变量或是方法的参数；成员变量可以被 public,private,static 等修饰符所修饰，而局部变量不能被访问控制修饰符及 static 所修饰；但是，成员变量和局部变量都能被 final 所修饰；
2. 从变量在内存中的存储方式来看，成员变量是对象的一部分，而对象存在于堆内存，局部变量存在于栈内存
3. 从变量在内存中的生存时间上看，成员变量是对象的一部分，它随着对象的创建而存在，而局部变量随着方法的调用而自动消失。
4. 成员变量如果没有被赋初值，则会自动以类型的默认值而赋值（一种情况例外被 final 修饰的成员变量也必须显示地赋值）；而局部变量则不会自动赋值。

## 构造方法有哪些特性

1. 名字与类名相同；
2. 没有返回值，但不能用void声明构造函数；
3. 生成类的对象时自动执行，无需调用。

## 静态方法和实例方法有何不同

1.在外部调用静态方法时，可以使用"类名.方法名"的方式，也可以使用"对象名.方法名"的方式。而实例方法只有后面这种方式。也就是说，调用静态方法可以无需创建对象。

2.静态方法在访问本类的成员时，只允许访问静态成员（即静态成员变量和静态方法），而不允许访问实例成员变量和实例方法；实例方法则无此限制

## 对象的相等与指向他们的引用相等，两者有什么不同

对象的相等，比的是内存中存放的内容是否相等。而引用相等，比较的是他们指向的内存地址是否相等。

## == 与 equals(重要)

**==** : 它的作用是判断两个对象的地址是不是相等。即，判断两个对象是不是同一个对象。(基本数据类型==比较的是值，引用数据类型==比较的是内存地址)

**equals()** : 它的作用也是判断两个对象是否相等。但它一般有两种使用情况：

- 情况1：类没有覆盖 equals() 方法。则通过 equals() 比较该类的两个对象时，等价于通过“==”比较这两个对象。
- 情况2：类覆盖了 equals() 方法。一般，我们都覆盖 equals() 方法来两个对象的内容相等；若它们的内容相等，则返回 true (即，认为这两个对象相等)。

**说明：**

- String 中的 equals 方法是被重写过的，因为 object 的 equals 方法是比较的对象的内存地址，而 String 的 equals 方法比较的是对象的值。
- 当创建 String 类型的对象时，虚拟机会在常量池中查找有没有已经存在的值和要创建的值相同的对象，如果有就把它赋给当前引用。如果没有就在常量池中重新创建一个 String 对象。

## hashCode 与 equals（重要）

面试官可能会问你：“你重写过 hashcode 和 equals 么，为什么重写equals时必须重写hashCode方法？”
hashCode（）介绍
hashCode() 的作用是获取哈希码，也称为散列码；它实际上是返回一个int整数。这个哈希码的作用是确定该对象在哈希表中的索引位置。hashCode() 定义在JDK的Object.java中，这就意味着Java中的任何类都包含有hashCode() 函数。
散列表存储的是键值对(key-value)，它的特点是：能根据“键”快速的检索出对应的“值”。这其中就利用到了散列码！（可以快速找到所需要的对象）
为什么要有 hashCode
我们以“HashSet 如何检查重复”为例子来说明为什么要有 hashCode：
当你把对象加入 HashSet 时，HashSet 会先计算对象的 hashcode 值来判断对象加入的位置，同时也会与其他已经加入的对象的 hashcode 值作比较，如果没有相符的hashcode，HashSet会假设对象没有重复出现。但是如果发现有相同 hashcode 值的对象，这时会调用 equals（）方法来检查 hashcode 相等的对象是否真的相同。如果两者相同，HashSet 就不会让其加入操作成功。如果不同的话，就会重新散列到其他位置。
hashCode（）与equals（）的相关规定

1.如果两个对象相等，则hashcode一定也是相同的
2.两个对象相等,对两个对象分别调用equals方法都返回true
3.两个对象有相同的hashcode值，它们也不一定是相等的
4.因此，equals 方法被覆盖过，则 hashCode 方法也必须被覆盖
5.hashCode() 的默认行为是对堆上的对象产生独特值。如果没有重写 hashCode()，则该 class 的两个对象无论如何都不会相等（即使这两个对象指向相同的数据）

## final、static、this、super

**final关键字**

final关键字主要用在三个地方：变量、方法、类。

**static** **关键字**

1. 修饰成员变量和成员方法被
2. 静态代码块
3. 静态内部类

**this 关键字**

在上面的示例中，this关键字用于两个地方：

- this.employees.length：访问类Manager的当前实例的变量。
- this.report（）：调用类Manager的当前实例的方法。

**super** **关键字**

super关键字用于从子类访问父类的变量和方法。 

**使用** **this** **和** **super** **要注意的问题：**

- super 调用父类中的其他构造方法时，调用时要放在构造方法的首行！this 调用本类中的其他构造方法时，也要放在首行。
- this、super不能用在static方法中。

**简单解释一下：**

被 static 修饰的成员属于类，不属于单个这个类的某个对象，被类中所有对象共享。而 this 代表对本类对象的引用，指向本类对象；而 super 代表对父类对象的引用，指向父类对象；所以， **this**和super是属于对象范畴的东西，而静态方法是属于类范畴的东西

## Java 中的异常处理

Java异常类层次结构图

![Javaå¼å¸¸ç±»å±æ¬¡ç»æå¾](https://camo.githubusercontent.com/27aa104d93ba0738be0f3d2e7d5b096c1619d12d/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d322f457863657074696f6e2e706e67)

在以下4种特殊情况下，finally块不会被执行：

1.在finally语句块中发生了异常。
2.在前面的代码中用了System.exit()退出程序。
3.程序所在的线程死亡。
4.关闭CPU。

## 获取用键盘输入常用的的两种方法

方法1：通过 Scanner

```java
Scanner input = new Scanner(System.in);
String s  = input.nextLine();
```

方法2：通过 BufferedReader

```Java
BufferedReader input = new BufferedReader(new InputStreamReader(System.in)); 
String s = input.readLine();
```

## Io

![IO-æä½æ¹å¼åç±»](https://camo.githubusercontent.com/639ec442b39898de071c3e4fd098215fb48f11e9/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d362f494f2d2545362539332538442545342542442539432545362539362542392545352542432538462545352538382538362545372542312542422e706e67)

### 什么是IO流？

它是一种数据的流从源头流到目的地。比如文件拷贝，输入流和输出流都包括了。输入流从文件中读取数据存储到进程(process)中，输出流从进程中读取数据然后写入到目标文件。

### 字节流和字符流的区别。

字节流是以字节为单位写入写出；字符流是以字符为单位写入写出。

### Java中流类的超类主要由那些？

java.io.InputStream；java.io.OutputStream；java.io.Reader；java.io.Writer

### FileInputStream和FileOutputStream是什么？

这是在拷贝文件操作的时候，经常用到的两个类。在处理小文件的时候，它们性能表现还不错，在大文件的时候，最好使用BufferedInputStream (或 BufferedReader) 和 BufferedOutputStream (或 BufferedWriter)

### 字节流和字符流，你更喜欢使用拿一个？

个人来说，更喜欢使用字符流，因为他们更新一些。许多在字符流中存在的特性，字节流中不存在。比如使用BufferedReader而不是BufferedInputStreams或DataInputStream，使用newLine()方法来读取下一行，但是在字节流中我们需要做额外的操作。

### System.out.println()是什么？

println是PrintStream的一个方法。out是一个静态PrintStream类型的成员变量，System是一个java.lang包中的类，用于和底层的操作系统进行交互。

### 在文件拷贝的时候，那一种流可用提升更多的性能？

在字节流的时候，使用BufferedInputStream和BufferedOutputStream。  在字符流的时候，使用BufferedReader 和 BufferedWriter

##  BIO,NIO,AIO,IO多路复用

blocking-IO：等待数据的过程中进程是阻塞的；

No-blocking-IO：等待数据的过程中不断的主动询问数据好了没有；

I/O多路复用：建立一个代理（select，epoll）监控请求，没请求则阻塞;

信号驱动IO：当数据报准备好读取时，内核就为该进程产生一个SIGIO信号

select和epoll最大的区别就是：select只是告诉你一定数目的流有事件了，至于哪个流有事件，还得你一个一个地去轮询，而epoll会把发生的事件告诉你，通过发生的事件，就自然而然定位到哪个流了。 

asynchronous I/O：不需要等待数据，数据好了会自动通知

![è¿éåå¾çæè¿°](https://img-blog.csdn.net/20151119165257068)

## IO模型对应的问题

### （1）blocking和non-blocking的区别

　　调用blocking IO会一直block住对应的进程直到操作完成，而non-blocking IO在kernel还准备数据的情况下会立刻返回。

### （2）synchronous IO和asynchronous IO的区别

　　在说明synchronous IO和asynchronous IO的区别之前，需要先给出两者的定义。POSIX的定义是这样子的：
　　　　- A synchronous I/O operation causes the requesting process to be blocked until that I/O operation completes;
　　　　- An asynchronous I/O operation does not cause the requesting process to be blocked;

　　两者的区别就在于synchronous IO做”IO operation”的时候会将process阻塞。按照这个定义，之前所述的blocking IO，non-blocking IO，IO multiplexing都属于synchronous IO。

　　有人会说，non-blocking IO并没有被block啊。这里有个非常“狡猾”的地方，定义中所指的”IO operation”是指真实的IO操作，就是例子中的recvfrom这个system call。non-blocking IO在执行recvfrom这个system call的时候，**如果kernel的数据没有准备好，这时候不会block进程**。但是，当**kernel中数据准备好的时候，recvfrom会将数据从kernel拷贝到用户内存中，这个时候进程是被block了**，在这段时间内，进程是被block的。

　　而asynchronous IO则不一样，当进程发起IO 操作之后，就直接返回再也不理睬了，直到kernel发送一个信号，告诉进程说IO完成。在这整个过程中，进程完全没有被block。

### （**3**）non-blocking IO和asynchronous IO的区别

　　可以发现non-blocking IO和asynchronous IO的区别还是很明显的。

　　--在non-blocking IO中，虽然进程大部分时间都不会被block，但是它**仍然要求进程去主动的check**，并且当数据准备完成以后，也需要**进程主动的再次调用recvfrom来将数据拷贝到用户内存**。

　　--而asynchronous IO则完全不同。它就像是用户进程将整个IO操作交给了他人（kernel）完成，然后他人做完后发信号通知。在此期间，**用户进程不需要去检查IO操作的状态，也不需要主动的去拷贝数据。** 

## 什么是RESTful架构：

（1）每一个URI代表一种资源；

（2）客户端通过四个HTTP动词，对服务器端资源进行操作;

## comparator接口

```java
是用来实现集合中元素的比较、排序的。
Collections.sort(stus, (s1,s2)->(s1.getAge()-s2.getAge()));
Collections.sort(stus,new Comparator<Student>() {
			@Override
			public int compare(Student s1, Student s2) {
				int flag;
				// 首选按年龄升序排序
				flag = s1.getAge()-s2.getAge();
				if(flag==0){
					// 再按学号升序排序
					flag = s1.getNum()-s2.getNum();
				}
				return flag;
			}
		});
```

## Iterator和ListIterator的区别是什么？

- Iterator可用来遍历Set和List集合，但是ListIterator只能用来遍历List。
- Iterator对集合只能是前向遍历，ListIterator既可以前向也可以后向。
- ListIterator实现了Iterator接口，并包含其他的功能，比如：增加元素，替换元素，获取前一个和后一个元素的索引，等等。

# JAVA CONCURRENT

## 同步、异步与阻塞、非阻塞的理解

同步：执行一个方法，在没有得到结果之前，不能执行后续操作。

异步：执行一个方法，在没有得到结果之前，就可以继续执行后续操作

阻塞：线程执行一个方法，在没有执行完之前，只能持续等待；

非阻塞：线程执行一个方法，在没有执行完之前，能进行其他操作；

## 什么是线程安全

如果一段代码可以保证被多线程访问后仍能保持正确性，就是线程安全的。

## Synchronized:

#### JVM实现底层：

同步代码块：同步语句块的实现使用的是monitorenter 和 monitorexit 指令
方法：方法表示ACC_SYNCHRONIZED；

Entry_Set     Wait_Set

#### synchronized关键字最主要的三种使用方式：

修饰实例方法，作用于当前对象实例加锁，进入同步代码前要获得当前对象实例的锁
修饰静态方法，作用于当前类对象加锁，进入同步代码前要获得当前类对象的锁 
修饰代码块

#### 谈谈 synchronized和ReenTrantLock 的区别

① 两者都是可重入锁
② synchronized 依赖于 JVM 而 ReenTrantLock 依赖于 API
③ ReenTrantLock 比 synchronized 增加了一些高级功能
相比synchronized，ReenTrantLock增加了一些高级功能。主要来说主要有三点：1.等待可中断；2.可实现公平锁；3.通知结果

③.synchroniezd执行完自动释放锁，ReenTrantLock需要unlock

#### Reentrantreadwitelock（读写锁）

读写锁，它维护了一个读锁和一个写锁，可以达到多个读线程可以共享的获取到锁，而此时写线程不能获取到锁，并且当写线程获取到锁时后续的读写都将被阻塞不能获取到锁。

## 各种锁

#### 悲观锁

总是假设最坏的情况，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会阻塞直到它拿到锁

#### 乐观锁

总是假设最好的情况，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号机制和CAS算法实现。

#### 自旋锁

自旋锁是指尝试获取锁的线程不会立即阻塞，而是采用循环的方式去尝试获取锁，这样的好处是减少线程上下文切换的消耗，缺点是循环会消耗CPU。

#### 偏向锁

偏向锁是指一段同步代码一直被一个线程所访问，那么该线程会自动获取锁。降低获取锁的代价。

#### 轻量级锁和重量级锁

轻量级锁是指当锁是偏向锁的时候，被另一个线程所访问，偏向锁就会升级为轻量级锁，其他线程会通过自旋的形式尝试获取锁，不会阻塞，提高性能。

重量级锁是指当锁为轻量级锁的时候，另一个线程虽然是自旋，但自旋不会一直持续下去，当自旋一定次数的时候，还没有获取到锁，就会进jianb入阻塞，该锁膨胀为重量级锁。重量级锁会让其他申请的线程进入阻塞，性能降低。

#### 锁消除

(有锁但不存在竞争，锁多余)：JVM编译优化，将不存在数据竞争的锁消除

## Volatile:

#### 讲一下Java内存模型

在 JDK1.2 之前，Java的内存模型实现总是从主存（即共享内存）读取变量，是不需要进行特别的注意的。而在当前的 Java 内存模型下，线程可以把变量保存本地内存（比如机器的寄存器）中，而不是直接在主存中进行读写。这就可能造成一个线程在主存中修改了一个变量的值，而另外一个线程还继续使用它在寄存器中的变量值的拷贝，造成数据的不一致。

![æ°æ®ä¸ä¸è´](https://camo.githubusercontent.com/2df61e9867d603bd3216c12851b2f7bcaec8847b/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d362f2545362539352542302545362538442541452545342542382538442545342542382538302545382538372542342e706e67)

要解决这个问题，就需要把变量声明为 volatile，这就指示 JVM，这个变量是不稳定的，每次使用它都到主存中进行读取。
说白了， volatile 关键字的主要作用就是保证变量的可见性然后还有一个作用是防止指令重排序。

![volatileå³é®å­çå¯è§æ§](https://camo.githubusercontent.com/9944baae059c325540072f4bb365a4d1591474c4/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d362f766f6c6174696c652545352538352542332545392539342541452545352541442539372545372539412538342545352538462541462545382541372538312545362538302541372e706e67)

#### 说说 synchronized 关键字和 volatile 关键字的区别

synchronized关键字和volatile关键字比较
1.volatile关键字是线程同步的轻量级实现，所以volatile性能肯定比synchronized关键字要好。

2.但是volatile关键字只能用于变量而synchronized关键字可以修饰方法以及代码块。
3.多线程访问volatile关键字不会发生阻塞，而synchronized关键字可能会发生阻塞
4.volatile不能保证数据的原子性。synchronized关键字能保证。

## 线程池：

#### 为什么要用线程池？

（1）降低资源消耗。 
（2）提高响应速度。 当任务到达时，任务可以不需要的等到线程创建就能立即执行。
（3）提高线程的可管理性。 线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源，还会降低系统的稳定性，使用线程池可以进行统一的分配，调优和监控。

#### 常见的线程池：

①SingleThreadExecutor
单个线程的线程池，即线程池中每次只有一个线程工作，单线程串行执行任务
②FixedThreadExecutor(n)
固定数量的线程池，没提交一个任务就是一个线程，直到达到线程池的最大数量，然后后面进入等待队列直到前面的任务完成才继续执行
③CacheThreadExecutor（推荐使用）
可缓存线程池，当线程池大小超过了处理任务所需的线程，那么就会回收部分空闲（一般是60秒无执行）的线程，当有任务来时，又智能的添加新线程来执行。
④ScheduleThreadExecutor
定时任务线程池

ThreadPoolExecutor:

corePoolSize - 池中所保存的线程数，包括空闲线程。

maximumPoolSize-池中允许的最大线程数。

keepAliveTime - 当线程数大于核心时，此为终止前多余的空闲线程等待新任务的最长时间。

unit  keepAliveTime参数的时间单位

threadFactory 执行者创建新线程时使用的工厂

RejectedExecutionHandler类型的变量，表示线程池的饱和策略。

线程池提供了4种策略：
1.AbortPolicy：直接抛出异常，这是默认策略；

2.CallerRunsPolicy：用调用者所在的线程来执行任务；

3.DiscardOldestPolicy：丢弃阻塞队列中靠最前的任务，并执行当前任务；

4.DiscardPolicy：直接丢弃任务；

#### 线程池调优:

设置最大线程数，防止线程资源耗尽； 
使用有界队列，从而增加系统的稳定性和预警能力(饱和策略)； 
根据任务的性质设置线程池大小：CPU密集型任务(CPU个数个线程)，IO密集型任务(CPU个数两倍的线程)，混合型任务(拆分)。

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

## Atom:

#### JUC 包中的原子类是哪4类?

基本类型
使用原子的方式更新基本类型
AtomicInteger：整形原子类
AtomicLong：长整型原子类
AtomicBoolean ：布尔型原子类
数组类型
使用原子的方式更新数组里的某个元素
AtomicIntegerArray：整形数组原子类
AtomicLongArray：长整形数组原子类
AtomicReferenceArray ：引用类型数组原子类
引用类型
AtomicReference：引用类型原子类
AtomicStampedRerence：原子更新引用类型里的字段原子类
AtomicMarkableReference ：原子更新带有标记位的引用类型
对象的属性修改类型
AtomicIntegerFieldUpdater:原子更新整形字段的更新器
AtomicLongFieldUpdater：原子更新长整形字段的更新器
AtomicStampedReference ：原子更新带有版本号的引用类型。该类将整数值与引用关联起来，可用于解决原子的更新数据和数据的版本号，可以解决使用 CAS 进行原子更新时可能出现的 ABA 问题。

#### 讲讲 AtomicInteger 的使用

AtomicInteger 类常用方法
public final int get() //获取当前的值
public final int getAndSet(int newValue)//获取当前的值，并设置新的值
public final int getAndIncrement()//获取当前的值，并自增
public final int getAndDecrement() //获取当前的值，并自减
public final int getAndAdd(int delta) //获取当前的值，并加上预期的值
boolean compareAndSet(int expect, int update) //如果输入的数值等于预期值，则以原子方式将该值设置为输入值（update）
public final void lazySet(int newValue)//最终设置为newValue,使用 lazySet 设置之后可能导致其他线程在之后的一小段时间内还是可以读到旧的值。

#### AtomicInteger 类的使用示例

```java
使用 AtomicInteger 之后，不用对 increment() 方法加锁也可以保证线程安全。
class AtomicIntegerTest {
        private AtomicInteger count = new AtomicInteger();
      //使用AtomicInteger之后，不需要对该方法加锁，也可以实现线程安全。
        public void increment() {
                  count.incrementAndGet();
        }
       public int getCount() {
                return count.get();
        }
}
```



#### AtomicInteger 线程安全原理简单分析

AtomicInteger 类的部分源码：
    // setup to use Unsafe.compareAndSwapInt for updates（更新操作时提供“比较并替换”的作用）
    private static final Unsafe unsafe = Unsafe.getUnsafe();
    private static final long valueOffset;
    static {
        try {
            valueOffset = unsafe.objectFieldOffset
                (AtomicInteger.class.getDeclaredField("value"));
        } catch (Exception ex) { throw new Error(ex); }
    }
    private volatile int value;
AtomicInteger 类主要利用 CAS (compare and swap) + volatile 和 native 方法来保证原子操作，从而避免 synchronized 的高开销，执行效率大为提升。

#### CAS的原理:

CAS机制当中使用了3个基本操作数：内存地址V，旧的预期值A，要修改的新值B。

更新一个变量的时候，只有当变量的预期值A和内存地址V当中的实际值相同时，才会将内存地址V对应的值修改为B。

## AQS :

#### AQS核心思想

如果被请求的共享资源空闲，则将当前请求资源的线程设置为有效的工作线程，并且将共享资源设置为锁定状态。如果被请求的共享资源被占用，那么就需要一套线程阻塞等待以及被唤醒时锁分配的机制，这个机制AQS是用CLH队列锁实现的，即将暂时获取不到锁的线程加入到队列中。 

![AQSåçå¾](https://camo.githubusercontent.com/13db51afdebad2dac67a224d422f6f60c9b8d366/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d362f4151532545352538452539462545372539302538362545352539422542452e706e67)

#### AQS定义两种资源共享方式

Exclusive（独占）：只有一个线程能执行，如ReentrantLock。又可分为公平锁和非公平锁：
公平锁：按照线程在队列中的排队顺序，先到者先拿到锁
非公平锁：当线程要获取锁时，无视队列顺序直接去抢锁，谁抢到就是谁的
Share（共享）：多个线程可同时执行，如Semaphore/CountDownLatch。Semaphore、CountDownLatCh、 CyclicBarrier、ReadWriteLock 我们都会在后面讲到。

#### AQS 组件总结

Semaphore(信号量)-允许多个线程同时访问： synchronized 和 ReentrantLock 都是一次只允许一个线程访问某个资源，Semaphore(信号量)可以指定多个线程同时访问某个资源。
CountDownLatch （倒计时器）： CountDownLatch是一个同步工具类，它允许一个或多个线程一直等待，直到其他线程的将CountDownLatch减到零后再执行。
CyclicBarrier(循环栅栏)： 让一组线程到达一个屏障时被阻塞，直到最后一个线程到达屏障时，屏障才会开门，所有被屏障拦截的线程才会继续干活。

## 进程，线程含义和区别

![img](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1561231556646&di=b3b0b21f15248881f8bccc226886c7da&imgtype=0&src=http%3A%2F%2Faliyunzixunbucket.oss-cn-beijing.aliyuncs.com%2Fjpg%2Fae52919274a26a79bdbb052230ae747b.jpg%3Fx-oss-process%3Dimage%2Fresize%2Cp_100%2Fauto-orient%2C1%2Fquality%2Cq_90%2Fformat%2Cjpg%2Fwatermark%2Cimage_eXVuY2VzaGk%3D%2Ct_100)

进程是对运行时程序的封装，是系统进行资源调度和分配的基本单位，实现了操作系统的并发；
线程是进程的子任务，是CPU调度和分配的基本单位，实现进程内部的并发；
一个程序至少有一个进程，一个进程至少有一个线程，线程依赖于进程而存在；
进程拥有独立的内存单元，而多个线程共享进程的内存。

#### 实现线程的4种方法：

继承Thread类创建线程
实现Runnable接口创建线程
实现Callable接口通过FutureTask包装器来创建Thread线程
使用ExecutorService、Callable、Future实现有返回结果的线程

#### Java中如何停止一个线程？

异常法

thread. Interrupt()
if(this.interrupted())  throw new InterruptedException();

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

#### 如何在Java中创建线程安全的Singleton？

利用JVM的类加载和静态变量初始化特征来创建Singleton实例，或者是利用枚举类型来创建Singleton。

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
take         移除并返回队列头部的元素，没有则阻塞
add          增加一个元索，满则抛出一个异常
offer        添加一个元素，如果队列已满，则返回false
put           添加一个元素，如果队列满，则阻塞
element  返回队列头部的元素，如果队列为空，则抛出一个异常

peek        返回队列头部的元素，如果队列为空，则返回null

## ThreadLocal及其引发的内存泄露

当释放掉对threadlocal对象的强引用后，map里面的value没有被回收，但却永远不会被访问到了，因此ThreadLocal存在着内存泄露问题。在不使用该ThreadLocal对象时，及时调用该对象的remove方法去移除ThreadLocal.ThreadLocalMap中的对应Entry。

# 集合类

![img](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1561230416001&di=b3658fff659fa1f807eafcd17fca49aa&imgtype=0&src=http%3A%2F%2Fimg1.ph.126.net%2Fidk6ftGbx0cfymqkTU5ljA%3D%3D%2F6630188156398360777.png)

## HashMap

#### 底层数据结构分析

JDK1.8 之前 HashMap 底层是 数组和链表 结合在一起使用也就是 链表散列。HashMap 通过 key 的 hashCode 经过扰动函数处理过后得到 hash 值，然后通过 (n - 1) & hash 判断当前元素存放的位置

(n-1)&hash=hash%n，且速度更快；

使用扰动函数之后可以减少碰撞。

#### loadFactor加载因子

loadFactor太大导致查找元素效率低，太小导致数组的利用率低

#### threshold

threshold = capacity * loadFactor，当Size>=threshold的时候，那么就要考虑对数组的扩增了

#### put方法

![putæ¹æ³](https://camo.githubusercontent.com/725bb9953df54438fadb74a1bea180ed817b9e27/68747470733a2f2f757365722d676f6c642d63646e2e786974752e696f2f323031382f392f322f313635393862663735386337343765363f773d39393926683d36373926663d706e6726733d3534343836)

  JDK1.7 put插入节点到链表头，1.8到链表尾

#### HashMap 扩容原理

lo就是扩容后仍然在原地的元素链表

hi就是扩容后下标为 原位置+原数组容量 的元素链表，从而不需要重新计算hash。

此处只需要判断左边新增的那一位（右数第5位）是否为1即可判断此节点是留在原地还是移动去高位

## HashMap 和 Hashtable 的区别

线程是否安全： HashMap 是非线程安全的，HashTable 是线程安全的；HashTable 内部的方法基本都经过 synchronized 修饰。
效率： 因为线程安全的问题，HashMap 要比 HashTable 效率高一点。另外，HashTable 基本被淘汰，不要在代码中使用它；
对Null key 和Null value的支持： HashMap 中，null 可以作为键，这样的键只有一个，可以有一个或多个键所对应的值为 null。。但是在 HashTable 中 put 进的键值只要有一个 null，直接抛出 NullPointerException。
初始容量大小和每次扩充容量大小的不同 ： ①创建时如果不指定容量初始值，Hashtable 默认的初始大小为11，之后每次扩充，容量变为原来的2n+1。HashMap 默认的初始化大小为16。之后每次扩充，容量变为原来的2倍。②创建时如果给定了容量初始值，那么 Hashtable 会直接使用你给定的大小，而 HashMap 会将其扩充为2的幂次方大小
底层数据结构： JDK1.8 以后的 HashMap 在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为8）时，将链表转化为红黑树，以减少搜索时间。Hashtable 没有这样的机制。

## HashMap 的长度为什么是2的幂次方

hash%length==hash&(length-1)的前提是 length 是2的 n 次方

## HashSet 和 HashMap 区别

| HashMap                                                | HashSet                                                      |
| ------------------------------------------------------ | ------------------------------------------------------------ |
| 实现了Map接口                                          | 实现Set接口                                                  |
| 存储键值对                                             | 仅存储对象                                                   |
| 调用put（）向map中添加元素                             | 调用add（）方法向Set中添加元素                               |
| HashMap使用键（Key）计算Hashcode                       | HashSet使用成员对象来计算hashcode值，对于两个对象来说hashcode可能相同，所以equals()方法用来判断对象的相等性，如果两个对象不同的话，那么返回false |
| HashMap相对于HashSet较快，因为它是使用唯一的键获取对象 | HashSet较HashMap来说比较慢                                   |

## ConcurrentHashMap 和 Hashtable 的区别

JDK1.7
首先将数据分为一段一段的存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据时，其他段的数据也能被其他线程访问。

ConcurrentHashMap 是由 Segment 数组结构和 HashEntry 数组结构组成。

Segment 实现了 ReentrantLock,所以 Segment 是一种可重入锁，扮演锁的角色。HashEntry 用于存储键值对数据。

JDK1.8 （上面有示意图）
ConcurrentHashMap取消了Segment分段锁，采用CAS和synchronized来保证并发安全。数据结构跟HashMap1.8的结构类似，数组+链表/红黑二叉树。

synchronized只锁定当前链表或红黑二叉树的首节点，这样只要hash不冲突，就不会产生并发，效率又提升N倍。

HashTable: 

![img](https://camo.githubusercontent.com/60920968c0e19878ca2a3a153cbe4872f950b571/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d362f486173685461626c652545352538352541382545382541312541382545392539342538312e706e67)

JDK1.7的ConcurrentHashMap： 

![JDK1.7çConcurrentHashMap](https://camo.githubusercontent.com/092aae16c3a38854b4cea8b7e42dc6720df4441f/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d362f436f6e63757272656e74486173684d61702545352538382538362545362541452542352545392539342538312e6a7067)

JDK1.8的ConcurrentHashMap： 

![JDK1.8çConcurrentHashMap](https://camo.githubusercontent.com/b823c5f2cf18e7e27da70409d2b5e18fed820364/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d362f4a444b312e382d436f6e63757272656e74486173684d61702d5374727563747572652e6a7067)

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

在我们学数据结构的时候就知道了线性表的顺序存储，插入删除元素的时间复杂度为**O（n）**,求表长以及增加元素，取第 i 元素的时间复杂度为**O（1）**

　 ArrayList 继承了AbstractList，实现了List。它是一个数组队列，提供了相关的添加、删除、修改、遍历等功能。

　　ArrayList 实现了**RandomAccess 接口**，即提供了随机访问功能。RandomAccess 是 Java 中用来被 List 实现，为 List 提供**快速访问功能**的。在 ArrayList 中，我们即可以通过元素的序号快速获取元素对象，这就是快速随机访问。

　　ArrayList 实现了**Cloneable 接口**，即覆盖了函数 clone()，**能被克隆**。

　　ArrayList 实现**java.io.Serializable 接口**，这意味着ArrayList**支持序列化**，**能通过序列化去传输**。

　　和 Vector 不同，**ArrayList 中的操作不是线程安全的**！所以，建议在单线程中才使用 ArrayList，而在多线程中可以选择 Vector 或者 CopyOnWriteArrayList。

以无参数构造方法创建 ArrayList 时，实际上初始化赋值的是一个空数组。当真正对数组进行添加元素操作时，才真正分配容量。即向数组中添加第一个元素时，数组容量扩为10。然后每次扩容为1.5倍

## CopyOnWriteArrayList

CopyOnWriteArrayList 类的所有可变操作（add，set等等）都是通过创建底层数组的新副本来实现的。当 List 需要被修改的时候，我并不修改原有内容，而是对原有数据进行一次复制，将修改的内容写入副本。写完之后，再将修改完的副本替换原来的数据，这样就可以保证写操作不会影响读操作了。



## LinkedList

LinkedList 底层使用的是双向链表数据结构（JDK1.6之前为循环链表，JDK1.7取消了循环。）

## Arraylist 与 LinkedList 异同

1. 是否保证线程安全： ArrayList 和 LinkedList 都是不同步的，也就是不保证线程安全；

2. 底层数据结构： Arraylist 底层使用的是Object数组；LinkedList 底层使用的是双向链表数据结构（JDK1.6之前为循环链表，JDK1.7取消了循环。注意双向链表和双向循环链表的区别：）； 详细可阅读JDK1.7-LinkedList循环链表优化

3. 插入和删除是否受元素位置的影响： ① ArrayList 采用数组存储，所以插入和删除元素的时间复杂度受元素位置的影响。 比如：执行add(E e) 方法的时候， ArrayList 会默认在将指定的元素追加到此列表的末尾，这种情况时间复杂度就是O(1)。但是如果要在指定位置 i 插入和删除元素的话（add(int index, E element) ）时间复杂度就为 O(n-i)。因为在进行上述操作的时候集合中第 i 和第 i 个元素之后的(n-i)个元素都要执行向后位/向前移一位的操作。 ② LinkedList 采用链表存储，所以插入，删除元素时间复杂度不受元素位置的影响，都是近似 O（1）而数组为近似 O（n）。

4. 是否支持快速随机访问： LinkedList 不支持高效的随机元素访问，而 ArrayList 支持。快速随机访问就是通过元素的序号快速获取元素对象(对应于get(int index) 方法)。

5. 内存空间占用： ArrayList的空 间浪费主要体现在在list列表的结尾会预留一定的容量空间，而LinkedList的空间花费则体现在它的每一个元素都需要消耗比ArrayList更多的空间（因为要存放直接后继和直接前驱以及数据）。

   ####  list 的遍历方式选择：

   实现了RandomAccess接口的list，优先选择普通for循环 ，其次foreach,
   未实现RandomAccess接口的list， 优先选择iterator遍历（foreach遍历底层也是通过iterator实现的），大size的数据，千万不要使用普通for循环

   ## 集合框架底层数据结构总结

   1. List
       Arraylist：数组（查询快,增删慢 线程不安全,效率高 ）
       Vector：数组（查询快,增删慢 线程安全,效率低 ）
       LinkedList：链表（查询慢,增删快 线程不安全,效率高 ）
   2. Set
       HashSet（无序，唯一）:哈希表或者叫散列集(hash table)
       LinkedHashSet：链表和哈希表组成 。 由链表保证元素的排序 ， 由哈希表证元素的唯一性
       TreeSet（有序，唯一）：红黑树(自平衡的排序二叉树。)
   - Map
     HashMap：基于哈希表的Map接口实现（哈希表对键进行散列，Map结构即映射表存放键值对）
     LinkedHashMap:HashMap 的基础上加上了链表数据结构
     HashTable:哈希表
     TreeMap:红黑树（自平衡的排序二叉树）

   

# 设计模式

## 创建型模式

#### 单例模式：

**保证类只有一个实例，并提供一个访问他的全局访问点**

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

#### 工厂模式：

工厂方法模式：**将创建对象的职责委托给了多个子类中的一个**

抽象方法模式：**将工厂的创建能力拓展到产品族**

#### 建造者模式：

**建造者模式仅仅关心构造一个完整复杂产品的步骤，而不关心生产细节**

#### 原型模式：

**通过“复制”来获得对象**

## 结构性模式

装饰者模式：**重点在于功能的扩展，添加额外的职责**.

适配器模式：**使得原本不兼容，不能够一起工作的那些类能够一起工作**

外观模式：**原本兼容的类组合在一起更加简单地工作**

代理模式：**对真实对象进行隐藏，封装**

组合模式：**用于描述“整体-部分”的概念**

桥接模式：**将抽象与实现进行分离**

#### 代理模式：

动态代理和静态代理；

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

#### 外观模式：

![1553231396721](C:\Users\admin\AppData\Roaming\Typora\typora-user-images\1553231396721.png)

#### 组合模式：

```java
public class Employee {
	private String name;
    private String dept;
    private int salary;
    private List<Employee> subordinates;//部下
}
```

#### 桥接模式：

![img](https://img-my.csdn.net/uploads/201206/17/1339921135_8442.png)

## 行为型模式

#### 观察者模式：

发布订阅模式

```java
public class WechatServer implements Observerable {
    
    //注意到这个List集合的泛型参数为Observer接口，设计原则：面向接口编程而不是面向实现编程
    private List<Observer> list;
}
```

#### 迭代器模式：

迭代器模式就是分离了集合对象的遍历行为，抽象出一个迭代器类来负责

#### 策略模式：

准备一组算法，并将每一个算法封装起来，使得它们可以互换			