# J2EE

## JavaWeb三大组件（Servlet、Filter、Listener）

1、Servlet 
Servlet是用来处理客户端请求的动态资源

2、Filter 

filter主要负责拦截请求

3、Listener 

Listener就是监听器，它可以监听Application、Session、Request对象，当这些对象发生变化就会调用对应的监听方法。 

## Listener,Filter,Servlet执行顺序

listener->Filter->servlet(理发师)

## Servlet生命周期

Web容器加载Servlet并将其实例化后，Servlet生命周期开始，容器运行其init()方法进行Servlet的初始化；请求到达时调用Servlet的service()方法，service()方法会根据需要调用与请求对应的doGet或doPost等方法；当服务器关闭或项目被卸载时服务器会将Servlet实例销毁，此时会调用Servlet的destroy()方法。

# Jvm虚拟机

## 介绍下Java内存区域

![img](https://camo.githubusercontent.com/a66819fd82c6adfa69b368edf3c52b1fa9cdc89d/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d332f4a564de8bf90e8a18ce697b6e695b0e68daee58cbae59f9f2e706e67)

![img](https://camo.githubusercontent.com/0bcc6c01a919b175827f0d5540aeec115df6c001/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d334a617661e8bf90e8a18ce697b6e695b0e68daee58cbae59f9f4a444b312e382e706e67)

- Java堆：占据了虚拟机管理内存中最大的一块（没想到吧），唯一目的就是存放对象实例（与引用是两个概念），也是垃圾回收器主要管理的地方 

- 方法区：存储加载的类信息、常量区、静态变量、JIT（即时编译器）处理后的数据等，类的信息包含类的版本、字段、方法、接口等信息。需要注意是常量池就在方法区中，也是我们这次需要关注的地方。
  JDK1.7中JVM把String常量区从方法区中移除了；JDK1.8中JVM把String常量池移入了堆中

- 程序计数器:字节码解释器工作时通过改变这个计数器的值来选取下一条需要执行的字节码指令，分支、循环、跳转、异常处理、线程恢复等功能都需要依赖这个计数器来完。

- Java 栈:局部变量表主要存放了编译器可知的各种数据类型（boolean、byte、char、short、int、float、long、double）、对象引用,每次方法调用的数据都是通过栈传递的。

- 本地方法栈:为虚拟机使用到的 Native 方法服务。

- ```java
  		  String str1 = "str";
  		  String str2 = "ing";
  		  String str3 = "str" + "ing";//常量池中的对象
  		  String str4 = str1 + str2; //在堆上创建的新的对象	  
  		  String str5 = "string";//常量池中的对象
  		  System.out.println(str3 == str4);//false
  		  System.out.println(str3 == str5);//true
  		  System.out.println(str4 == str5);//false
  ```
  
## 对象的访问定位的两种方式

![å¯¹è±¡çè®¿é®å®ä½-ä½¿ç¨å¥æ](https://camo.githubusercontent.com/04c82b46121149c8cc9c3b81e18967a5ce06353f/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d362f2545352541462542392545382542312541312545372539412538342545382541452542462545392539372541452545352541452539412545342542442538442d2545342542442542462545372539342541382545352538462541352545362539462538342e706e67)

![å¯¹è±¡çè®¿é®å®ä½-ç´æ¥æé](https://camo.githubusercontent.com/0ae309b058b45ee14004cd001e334355231b2246/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d362f2545352541462542392545382542312541312545372539412538342545382541452542462545392539372541452545352541452539412545342542442538442d2545372539422542342545362538452541352545362538432538372545392539322538382e706e67)

## 如何判断对象是否死亡（两种方法）。

引用计数法: 给对象中添加一个引用计数器，每当有一个地方引用它，计数器就加1；当引用失效，计数器就减1；任何时候计数器为0的对象就是不可能再被使用的。
可达性分析算法:通过一系列的称为 “GC Roots” 的对象作为起点,当一个对象到GC Roots没有任何引用链相连的话，则证明此对象是不可用的。

GC Roots：

1.栈中引用的对象；

2.方法区中的静态，常量引用对象；

3.本地方法栈中引用的对象；

## 判断对象死亡后，如何避免GC

**gc进行两次标记**，即第一次标记不在“关系网”中的对象。**第二次的话就要先判断该对象有没有实现finalize()方法了**，如果没有实现就直接判断该对象可回收；如果实现了就会先放在一个队列中

## Stop-The-World原因

java线程要停止才能彻底清除干净，否则会影响gc线程的处理效率增加gc线程负担

##  简单的介绍一下强引用、软引用、弱引用、虚引用（虚引用与软引用和弱引用的区别、使用软引用能带来的好处）。

#### 1．强引用

垃圾回收器绝不会回收它。当内存空间不足，Java虚拟机宁愿抛出OutOfMemoryError错误，使程序异常终止，也不会靠随意回收具有强引用的对象来解决内存不足问题。

#### 2．软引用（SoftReference）

如果内存空间足够，垃圾回收器就不会回收它，如果内存空间不足了，就会回收这些对象的内存。只要垃圾回收器没有回收它，该对象就可以被程序使用。软引用可用来实现内存敏感的高速缓存。

#### 3．弱引用（WeakReference）

一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存。

#### 4．虚引用（PhantomReference）

"虚引用"顾名思义，就是形同虚设，与其他几种引用都不同，虚引用并不会决定对象的生命周期。如果一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收。

## 垃圾收集有哪些算法，各自的特点？

标记-清除算法
复制算法
标记-整理算法
分代收集算法

## HotSpot为什么要分为新生代和老年代？

这样划分的目的是为了使 JVM 能够更好的管理堆内存中的对象，包括内存的分配以及回收。

## 堆内存分配策略

![å åå­å¸¸è§åéç­ç¥ ](https://camo.githubusercontent.com/6b0bb14d82c24d5d4b0302c69a11589b3d887adc/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d362f2545352541302538362545352538362538352545352541442539382e6a7067)

## 什么对象进入老年代：

1.大对象；

2.长期存活的对象；

3.相同年龄的对象占了survivor空间的一半

## 并行和并发：

收集器中的并行（Parallel）语义：指多条垃圾收集线程并行工作，但此时用户线程仍处于等待状态
收集器中的并发（Concurrent）语义：指用户线程与垃圾收集线程同时执行（但不一定是并行的，可能会交替执行），用户程序在继续运行，而垃圾收集程序于另一个CPU上。

## 介绍一下收集器。

![img](https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=3841264175,3663926521&fm=15&gp=0.jpg)

•  Serial收集器（复制算法): 新生代单线程收集器，标记和清理都是单线程，优点是简单高效； 
•  Serial Old收集器 (标记-整理算法): 老年代单线程收集器，Serial收集器的老年代版本； 
•  ParNew收集器 (复制算法):新生代并行收集器，实际上是Serial收集器的多线程版本，在多核CPU环境下有着比Serial更好的表现； 
•  Parallel Scavenge收集器 (复制算法): 新生代并行收集器，追求高吞吐量，高效利用 CPU。
•  Parallel Old收集器 (标记-整理算法)： 老年代并行收集器，吞吐量优先，Parallel Scavenge收集器的老年代版本； 
•  CMS(Concurrent Mark Sweep)收集器（标记-清除算法）：老年代并行收集器，以获取最短回收停顿时间为目标的收集器，具有高并发、低停顿的特点，追求最短GC回收停顿时间。 
•  G1(Garbage First)收集器 (标记-整理算法)：Java堆并行收集器，G1收集器是JDK1.7提供的一个新收集器，G1收集器基于“标记-整理”算法实现，也就是说不会产生内存碎片。此外，G1收集器不同于之前的收集器的一个重要特点是：G1回收的范围是整个Java堆(包括新生代，老年代)，而前六种收集器回收的范围仅限于新生代或老年代。

![img](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1561827132&di=e409b6bd14bb4bc312c36c6227b9c9c7&imgtype=jpg&er=1&src=http%3A%2F%2Fimg2018.cnblogs.com%2Fblog%2F1591870%2F201903%2F1591870-20190317160744429-966296736.png)

## Minor Gc和Full GC 有什么不同呢？

Minor GC:从年轻代空间（包括 Eden 和 Survivor 区域）回收内存
Major GC:只收集老年代
Full GC:收集整个堆

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

![ç±»å è½½è¿ç¨-11.2kB](http://static.zybuluo.com/Rico123/ojhhtids41ivtuowfj74mkb2/%E7%B1%BB%E5%8A%A0%E8%BD%BD%E8%BF%87%E7%A8%8B)

## 类的实例化与类的初始化

- 类的实例化是指创建一个类的实例(对象)的过程；
- 类会在首次被“主动使用”时执行初始化，类的初始化是指为类中各个类成员(被static修饰的成员变量)赋初始值的过程，是类生命周期中的一个阶段。准备阶段，JVM为[静态变量](https://www.baidu.com/s?wd=静态变量&tn=44039180_cpr&fenlei=mv6quAkxTZn0IZRqIHckPjm4nH00T1Y4PHRdPj6Ym1TsnAPhrH6d0ZwV5Hcvrjm3rH6sPfKWUMw85HfYnjn4nH6sgvPsT6KdThsqpZwYTjCEQLGCpyw9Uz4Bmy-bIi4WUvYETgN-TLwGUv3EnHbvnjbvnHDdPW0vnj6sPHfvn0)[分配内存](https://www.baidu.com/s?wd=分配内存&tn=44039180_cpr&fenlei=mv6quAkxTZn0IZRqIHckPjm4nH00T1Y4PHRdPj6Ym1TsnAPhrH6d0ZwV5Hcvrjm3rH6sPfKWUMw85HfYnjn4nH6sgvPsT6KdThsqpZwYTjCEQLGCpyw9Uz4Bmy-bIi4WUvYETgN-TLwGUv3EnHbvnjbvnHDdPW0vnj6sPHfvn0)空间

## 对类加载器有了解吗？什么是双亲委派模型？

![ClassLoader](https://camo.githubusercontent.com/4311721b0968c1b9fd63bdc0acf11d7358a52ff6/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d362f636c6173736c6f616465725f5750532545352539422542452545372538392538372e706e67)

## 双亲委派模型

AppClassLoader应用类加载器,又称为系统类加载器,负责在JVM启动时,加载来自在命令java中的classpath的jar包

ExtClassLoader称为扩展类加载器，主要负责加载Java的扩展类库,默认加载JAVA_HOME/jre/lib/ext/目录下的所有jar包

BootstrapClassLoader：Java类加载层次中最顶层的类加载器，负责加载JDK中的核心类库

## 如何破坏双亲委派模型

打破双亲委派机制则不仅要继承ClassLoader类，还要重写loadClass和findClass方法

沿用双亲委派机制自定义类加载器很简单，只需继承ClassLoader类并重写findClass方法即可。

## 热部署为什么要自定义类加载器

class文件被修改过，那么先卸载对应class类加载器，重新启动，然后就会重新加载类。

## tomcat为什么要自定义类加载器

一个web容器可能需要部署两个应用程序，不同的应用程序可能会依赖同一个第三方类库的不同版本，如果使用默认的类加载器机制，那么是无法加载两个相同类库的不同版本的，由全限定类名决定，并且只有一份。

web容器也有自己依赖的类库，不能于应用程序的类库混淆。

#### 调优工具：

jstack：通过 jstack 命令可以获取当前进程的所有线程信息，Java进程有多少线程，每个线程什么状态，是不是在等着锁

jps：显示当前所有java进程pid的命令

Jstat：对其堆的使用情况进行实时的命令行的统计

Jconsole：可以以图表化的形式显示各种数据

| 空间   | 命令行选项                      | 占用倍数                            |
| ------ | ------------------------------- | ----------------------------------- |
| Java堆 | -Xms和-Xmx                      | 3~4倍FullGC后的老年代空间占用量     |
| 永久代 | -xx：PermSize和-xx：MaxPermSize | 1.2~1.5倍FullGC后的永久代空间占用量 |
| 新生代 | -Xmn                            | 1~1.5倍FullGC后的老年代空间占用量   |
| 老年代 | Java堆大小减新生代大小          | 2~3倍FullGC后的老年代空间占用量     |

### 如何排查cpu100%

1. Java 内存不够或溢出导致GC overhead问题, GC overhead 导致的CPU 100%问题;
2. 死循环问题. 如常见的HashMap被多个线程并发使用导致的死循环, 或者上面例子中的死循环;
3. 线程过多。

### 如何查找内存泄漏

将进程堆的情况dump下来，使用分析工具进行分析，找到发生内存占用过大的对象。

# Linux

## 常用命令：

#### 目录切换命令

`cd /`： 切换到系统根目录

`cd ~`： 切换到用户主目录

#### 目录的操作命令（增删改查）

`mkdir 目录名称`： 增加目录

`ls或者ll`（ll是ls -l的缩写，ll命令以看到该目录下的所有目录和文件的详细信息）：查看目录信息

`find 目录 参数`： 寻找文件	find . -name test（查找当前目录）find . -name "*.txt"

`mv` 目录名称 目录的新位置： 移动目录的位置---剪切（改）

`cp` -r 目录名称 目录拷贝的目标位置： 拷贝目录（改），-r代表递归拷贝

`rm` [-rf] 目录: 删除目录（删）

`chown`：利用 chown 将指定文件的拥有者改为指定的用户或组   

`chmod`：Linux/Unix 的文件调用权限分为三级 : 文件拥有者、群组、其他。利用 chmod 可以藉以控制文件如何被他人所调用，r=4，w=2，x=1；

#### 文件的操作命令

`cat`： 只能显示最后一屏内容

`more`： 一页一页浏览

`less`： 使用 less 可以随意浏览文件，而 more 仅能向前移动，却不能向后移动

`head`: 	head -n 10

`tail` ： tail -n 10

`vim` 文件： 修改文件的内容（改）	/jdbc(vi中查找)

`touch` 文件名称`: 文件的创建（增）

`rm` -rf 文件： 删除文件（删）

#### 压缩文件的操作命令

打包并压缩文件： tar -zcvf 打包压缩后的文件名 要打包压缩的文件

解压压缩包：tar -xvf test.tar.gz

#### 其他常用命令

ps -ef|grep 

kill -9 

top P,M

`netstat` -nap

查看linux操作系统磁盘空间命令：df

查看某个端口是否被使用 lsof -i:6379

# Git

## git config

git config --system:系统中对所有用户都普遍适用的配置。

git config --global:用户目录下的配置文件只适用于该用户。

## 用户信息

git config --global user.name "John Doe"

git config --global user.email johndoe@example.com

要检查已有的配置信息，可以使用 `git config --list` 命令。

## git如何拉取指定分支的代码

git clone -b develop XXX 

## git 切换远程分支

git checkout -b dev origin/dev

## 取消已经暂存的文件

git reset HEAD <file>

git push -f

## 拉取主分支更新的代码

git stash

git pull origin master

git stash pop

## 合并分支：

分支提交更多：

![git-br-on-master](https://www.liaoxuefeng.com/files/attachments/919022533080576/0)

切换到master；

git merge dev;

## 解决冲突

分别提交后：

![git-br-feature1](https://www.liaoxuefeng.com/files/attachments/919023000423040/0)

切换到master ：git merge feature1;

修改冲突；

提交后：

![git-br-conflict-merged](https://www.liaoxuefeng.com/files/attachments/919023031831104/0)

切换到feature1：git merge master；

## 多人协作

git push失败，因为别人先push过一个新版本。

因此先git pull，但提示错误，原因是没有指定本地`dev`分支与远程`origin/dev`分支的链接。

git branch --set-upstream-to=origin/dev dev

然后pull成功，但是合并有冲突，需要手动解决。

解决后commit，然后push。

## Stash

git stash list

git stash pop

git stash apply stash@{0}

小结:  修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场。



git log --graph  分支合并图

git checkout 切换分支

git branch vv

git add -A

git config --global --edit

git push 和git push origin master(是否checkout到对应分支)；

















