#### 类型转换

int(1.555)

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

Golang中`map`的底层实现是一个散列表，因此实现`map`的过程实际上就是实现散表的过程。在这个散列表中，主要出现的结构体有两个，一个叫**hmap**(`a header for a go map`)，一个叫**bmap**(`a bucket for a Go map`，通常叫其`bucket`)。这两种结构的样子分别如下所示：

**hmap**:![img](https://static.studygolang.com/180826/09d0c94fc2946bba795ecc2ae0c97ac2.png)

图中有很多字段，但是便于理解`map`的架构，你只需要关心的只有一个，就是标红的字段：**buckets数组**。Golang的map中用于存储的结构是bucket数组。而bucket(即`bmap`)的结构是怎样的呢？

**bucket**：![img](https://img2018.cnblogs.com/blog/1199549/201906/1199549-20190622233814630-764863225.png)

 

相比于`hmap`，bucket的结构显得简单一些，标红的字段依然是“核心”，我们使用的`map`中的key和value就存储在这里。“高位哈希值”数组记录的是当前bucket中key相关的“索引”，稍后会详细叙述。还有一个字段是一个指向扩容后的bucket的指针，使得bucket会形成一个链表结构。例如下图：

![img](https://img2018.cnblogs.com/blog/1199549/201906/1199549-20190622233345395-309350836.png)

 

由此看出`hmap`和`bucket`的关系是这样的：

![img](https://img2018.cnblogs.com/blog/1199549/201906/1199549-20190622232825554-644793812.png)

 

而bucket又是一个链表，所以，整体的结构应该是这样的：

![img](https://img2018.cnblogs.com/blog/1199549/201906/1199549-20190622233114658-1984741865.png)

 

哈希表的特点是会有一个哈希函数，对你传来的key进行哈希运算，得到唯一的值，一般情况下都是一个数值。Golang的`map`中也有这么一个哈希函数，也会算出唯一的值，对于这个值的使用，Golang也是很有意思。

Golang把求得的值按照用途一分为二：高位和低位。

![这里写图片描述](https://static.studygolang.com/180826/f78e6681a0c9baf8aa677290443b4b7d.png)

如图所示，蓝色为高位，红色为低位。 然后低位用于寻找当前key属于`hmap`中的哪个bucket，而高位用于寻找bucket中的哪个key。上文中提到：bucket中有个属性字段是“高位哈希值”数组，这里存的就是蓝色的高位值，用来声明当前bucket中有哪些“key”，便于搜索查找。 需要特别指出的一点是：我们`map`中的key/value值都是存到同一个数组中的。数组中的顺序是这样的:

![这里写图片描述](https://static.studygolang.com/180826/a662bfc57e0b63211f1f2783129969c6.png)

并不是key0/value0/key1/value1的形式，这样做的好处是：在key和value的长度不同的时候，可**以消除padding(内存对齐)带来的空间浪费**。

现在，我们可以得到Go语言`map`的整个的结构图了：(hash结果的低位用于选择把KV放在bmap数组中的哪一个bmap中，高位用于key的快速预览，用于快速试错)

![这里写图片描述](https://static.studygolang.com/180826/7d806b2a30f30d85e3ee65fb25929263.png)

**map的扩容**

当以上的哈希表增长的时候，Go语言会将bucket数组的数量扩充一倍，产生一个新的bucket数组，并将旧数组的数据迁移至新数组。

加载因子
判断扩充的条件，就是哈希表中的加载因子(即loadFactor)。

加载因子是一个阈值，一般表示为：散列包含的元素数 除以 位置总数。是一种“产生冲突机会”和“空间使用”的平衡与折中：加载因子越小，说明空间空置率高，空间使用率小，但是加载因子越大，说明空间利用率上去了，但是“产生冲突机会”高了。

每种哈希表的都会有一个加载因子，数值超过加载因子就会为哈希表扩容。
Golang的map的加载因子的公式是：map长度 / 2^B(这是代表bmap数组的长度，B是取的低位的位数)阈值是6.5。其中B可以理解为已扩容的次数。

当Go的map长度增长到大于加载因子所需的map长度时，Go语言就会将产生一个新的bucket数组，然后把旧的bucket数组移到一个属性字段oldbucket中。注意：并不是立刻把旧的数组中的元素转义到新的bucket当中，而是，只有当访问到具体的某个bucket的时候，会把bucket中的数据转移到新的bucket中。

如下图所示：当扩容的时候，Go的map结构体中，会保存旧的数据，和新生成的数组

 ![img](https://img2018.cnblogs.com/blog/1199549/201906/1199549-20190622231255790-1061374322.png)

 

上面部分代表旧的有数据的bucket，下面部分代表新生成的新的bucket。蓝色代表存有数据的bucket，橘黄色代表空的bucket。
扩容时map并不会立即把新数据做迁移，而是当访问原来旧bucket的数据的时候，才把旧数据做迁移，如下图：

 ![img](https://img2018.cnblogs.com/blog/1199549/201906/1199549-20190622231311335-649924449.png)

 

注意：这里并不会直接删除旧的bucket，而是把原来的引用去掉，利用GC清除内存。

map中数据的删除
如果理解了map的整体结构，那么查找、更新、删除的基本步骤应该都很清楚了。这里不再赘述。
值得注意的是，找到了map中的数据之后，针对key和value分别做如下操作：

```
`1、如果``key``是一个指针类型的，则直接将其置为空，等待GC清除；``2、如果是值类型的，则清除相关内存。``3、同理，对``value``做相同的操作。``4、最后把key对应的高位值对应的数组index置为空。`
```

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

```
ch1 := make(chan string)
var ch1 chan string
ch1 = make(chan string)
ch <- int1//send
int2 = <- ch//receive
ch1 := make(chan string, buf)
```

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