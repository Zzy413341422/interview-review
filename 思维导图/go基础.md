## 闭包

![img](C:\Users\admin\Desktop\all the web\interview-review\md\77.png)

#### 类型转换

## 基础类型

go语言类型转换没有隐式，只有强制转换。(例如int8+int64,int+string都会编译错误)

#### 整形
有符号整形 int8 int16 int32 int64 默认值 0
无符号整形 uint8 uint16 uint32 uint64 默认值 0
特殊整形 int uint byte rune uintptr 默认值 0

#### 浮点型
浮点型数 float32 float64 默认值 0.0
复数类型 complex64 complex128 默认值 0+0i
#### 布尔类型
true false 默认值 false
#### 字符串类型
 string 默认值 ""
#### 复合类型
- 数组类型 `[SIZE]TYPE` 默认值根据数组类型变化而变化 如 `[3]int` 为 `[0,0,0]`
- 结构体类型 `struct` 默认值根据随结构体内部类型变化而变化，如下默认值为`{ 0}` 即 `Name`为`""` `Age`为`0`
#### **引用类型**
- 指针 `*TYPE` 默认值 `nil`
- 切片 `[]TYPE` 默认值 `nil`
- 字典 `map[TYPE][TYPE]` 默认值 `nil`
- 通道 `chan` 默认值 `nil`
- 函数 `func` 默认值 `nil`
#### 接口类型
接口 interface 默认值 nil

#### 指针

x := 1
p := &x         // p, of type *int, points to x
*p = 2          // equivalent to x = 2=

## 数组

```
var arr1 [5]int
var arrAge = [5]int{18, 20, 15, 22, 16}
var arr1 = new([5]int)
arr :=[5]int{18, 20, 15, 22, 16}
```

### range会复制对象，而不是不是直接在原对象上操作。

### 使用range迭代遍历引用类型时，底层的数据不会被复制。

## 切片

```
var s = []int{1,2,3}
var slice1 []int = arr1[2:5]
slice1 = slice1[0:4]
var slice1 []int = make([]int, 10)
var v []int = make([]int, 10, 50)
v := make([]int, 10, 50)
c := []bytes(s) //从字符串生成字节切片
substr := str[start:end]//获取字符串的某一部分:不包括end
for ix, value := range slice1(arr) {
    ...
}
func addStruct(list *[]student)  {

}
b = append(a[:0:0], a...)//完美地克隆一个切片
```

#### 扩容原理：

```text
type slice struct {
	array unsafe.Pointer // 数组指针
	len   int // 长度 
	cap   int // 容量
}
```

如果底层结构的数组容量足够，改变 slice 的 len 即可，slice 地址不会改。

如果底层结构的数组容量不够，Go 会自动帮助创建新的数组，并将 s 指向新创建的数组，但是同样，slice 本身的地址并没有改变，只是改变了地址中的内容。

是超出切片cap限制，而非底层数组长度限制，因为cap可小于数组长度。新分配数组长度是原cap的2倍，而非原数组的2倍。并非总是2倍，对于较大的切片，会尝试扩容1/4，以节约内存。

## Map

```
var map1 map[string]int
mapCreated := make(map[string]float32)
if _, ok := map1[key1]; ok {
    // ...
}
for k,v:=range studentMap{
	fmt.Println(k)
	fmt.Println(v)
}
func addStruct(stuMap map[string]int)  {
	// ...
}
```

**map**是Go语言中基础的数据结构，在日常的使用中经常被用到。但是它底层是如何实现的呢？

总体来说golang的map是hashmap，是使用数组+链表的形式实现的，使用拉链法消除hash冲突。

golang的map由两种重要的结构，hmap和bmap(下文中都有解释)，主要就是hmap中包含一个指向bmap数组的指针，key经过hash函数之后得到一个数，这个数低位用于选择bmap(当作bmap数组指针的下表)，高位用于放在bmap的[8]uint8数组中，用于快速试错。然后一个bmap可以指向下一个bmap(拉链)。

Golang中`map`的底层实现是一个散列表，因此实现`map`的过程实际上就是实现散表的过程。在这个散列表中，主要出现的结构体有两个，一个叫**hmap**(`a header for a go map`)，一个叫**bmap**(`a bucket for a Go map`，通常叫其`bucket`)。

## 结构体

```
type struct1 struct {
    i1  int
    f1  float32
    str string
}
ms := new(struct1)//pointer
ms := struct1{10, 15.5, "Chris"}//object
ms := &struct1{10, 15.5, "Chris"}//pointer
//pointer value change
ms.thingOne = "hello"
ms.thingTwo = 1
func (a *sparseMatrix) Add(b Matrix) Matrix//method
//结构体继承
type Camera struct{}

type Phone struct{}

type CameraPhone struct {
    Camera
    Phone
}
```

## 接口

只要类型实现了接口中的方法，它就实现了此接口。

```
type Shaper interface {
    Area() float32
}
type Square struct {
    side float32
}
func (sq *Square) Area() float32 {
    return sq.side * sq.side
}
```

## 协程

轻量级线程

非抢占式多任务处理，由协程主动交出控制层

编译器层面的多任务（java thread操作系统级别）

多个协程可能在一个或多个线程上运行

goroutine阻塞后调度器自动切换，但不能保证切换

起一个go程大概只需要4kb的内存，起一个Java线程需要1.5MB的内存；go程的调度在用户态非常轻量，Java线程的切换成本比较高。接着问为啥成本比较高？因为Java线程的调度需要在用户态和内核态切换所以成本高？为啥在用户态和内核态之间切换调度成本比较高？简单说了下内核态和用户态的定义。接着问，还是没有明白为啥成本高？心里瞬间崩溃，没完没了了呀，OS这块依旧是痛呀，支支吾吾半天放弃了。

```
ch1 := make(chan string)
var ch1 chan string
ch1 = make(chan string)
ch <- int1//send
int2 = <- ch//receive
ch1 := make(chan string, buf)
```

## goroutine和线程区别

从调度上看，goroutine的调度开销远远小于线程调度开销。

OS的线程由OS内核调度，每隔几毫秒，一个硬件时钟中断发到CPU，CPU调用一个调度器内核函数。这个函数暂停当前正在运行的线程，把他的寄存器信息保存到内存中，查看线程列表并决定接下来运行哪一个线程，再从内存中恢复线程的注册表信息，最后继续执行选中的线程。这种线程切换需要一个完整的上下文切换：即保存一个线程的状态到内存，再恢复另外一个线程的状态，最后更新调度器的数据结构。某种意义上，这种操作还是很慢的。

Go运行的时候包涵一个自己的调度器，这个调度器使用一个称为一个M:N调度技术，m个goroutine到n个os线程（可以用GOMAXPROCS来控制n的数量），Go的调度器不是由硬件时钟来定期触发的，而是由特定的go语言结构来触发的，他不需要切换到内核语境，所以调度一个goroutine比调度一个线程的成本低很多。

从栈空间上，goroutine的栈空间更加动态灵活。

每个OS的线程都有一个固定大小的栈内存，通常是2MB，栈内存用于保存在其他函数调用期间哪些正在执行或者临时暂停的函数的局部变量。这个固定的栈大小，如果对于goroutine来说，可能是一种巨大的浪费。作为对比goroutine在生命周期开始只有一个很小的栈，典型情况是2KB, 在go程序中，一次创建十万左右的goroutine也不罕见（2KB*100,000=200MB）。而且goroutine的栈不是固定大小，它可以按需增大和缩小，最大限制可以到1GB。

goroutine没有一个特定的标识。

在大部分支持多线程的操作系统和编程语言中，线程有一个独特的标识，通常是一个整数或者指针，这个特性可以让我们构建一个线程的局部存储，本质是一个全局的map，以线程的标识作为键，这样每个线程可以独立使用这个map存储和获取值，不受其他线程干扰。

goroutine中没有可供程序员访问的标识，原因是一种纯函数的理念，不希望滥用线程局部存储导致一个不健康的超距作用，即函数的行为不仅取决于它的参数，还取决于运行它的线程标识。

## 函数

#### 函数重载是不被允许的

#### 传递变长参数

```
func Greeting(prefix string, who ...string)
arr := []int{7,9,3,5,1}
x = func1(arr...)
```

#### 将函数作为参数

函数可以作为其它函数的参数进行传递，然后在其它函数内调用执行

```
func callback(y int, f func(int, int)) {
    f(y, 2) // this becomes Add(1, 2)
}
```

#### 匿名函数

```
func(x, y int) int { return x + y }
```

#### 闭包

```
func incr() func() int {
	var x int
	return func() int {
		x++
		return x
	}
}
i := incr() //i 就成为了一个闭包。全局变量i指向含有x的函数，让x逃逸了。
```

#### new(T) 为每个新的类型T分配一片内存，初始化为 0 并且返回类型为*T的内存地址：这种方法 返回一个指向类型为 T，值为 0 的地址的指针

#### make(T) **返回一个类型为 T 的初始值**

#### 值类型分别有：int系列、float系列、bool、string、数组和结构体

#### 引用类型有：指针、slice切片、管道channel、接口interface、map、函数等

内置函数new按指定类型长度分配零值内存，返回指针，并不关心类型内部构造和初始化方式。而引用类型则必须使用make函数创建，编译器会将make转换为目标类型专用的创建函数(或指令)，以确保完成全部内存分配和相关属性初始化。





- goroutine基于线程池的P:M:G协程模型
- channel基于生产者消费者模型的无锁队列
- net. conn基于epoll的异步io同步阻塞模型
- syscall基于操作系统的原生syscall能力
- gosched基于阻塞的协程调度
- gc基于三色标记法的并发gc模型
- io. reader/writer unix文件哲学升级版
- net/http基于goroutine的http服务器
- 开箱即用error基于c风格的if(erron ! = 0)错误处理机制
- panic传统的exception异常机制可配合coredumprecover可用于恢复异常的堆栈，
- 以进行排错map传统的hashmap
- 并发安全的hashmapslice
- 基于内存复用和读优化设计的数据结构
- defer函数返回前清理各种垃圾，防止内存泄露
- go tool asm go专用汇编，
- 可选性能优化手段cgo非并发安全的c调用能力，
- 可选性能优化手段unsafe非并发安全的指针调用，
- 可选性能优化手段reflect提供反射能力，
- 以实现有限的动态性atomic基于cpu原子操作的包装，
- 可实现cascontext基于channel的goroutine流程控制能力
- interface提供高级语言的抽象和多态能力闭包提供主流编程语言的闭包设计
- 逃逸分析提供主流编程语言的逃逸优化能力指针提供并发安全的指针
- 非并发安全的指针pprof自带的性能分析工具用于调优和查错