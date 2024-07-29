##### 2024.7.15---Go指南：

###### 1.方法（带接收者参数的函数）

```go
package main

import (
    "fmt"
    "math"
)

type Vertex struct {
    x, y float64
}

func (v Vertex) Abs() float64 {
    return math.Sqrt(v.x*v.x + v.y*v.y)
}

func main() {
    v := Vertex{3, 4}
    fmt.Println(v.Abs())
}

```

在我理解中，方法就是给对象添加了可以直接引用的函数，由于Go语言特性，一个方法不能有多个接收者参数，但是可以组合引用；

<mark>：type Myfloat float64，给float64添加别名Myfloat</mark>

```go
package main

import (
    "fmt"
    "math"
)

type MyFloat float64

func (f MyFloat) Abs() float64 {
    if f < 0 {
        return float64(-f)
    }
    return float64(f)
}

func main() {
    f := MyFloat(-math.Sqrt2)
    fmt.Println(f.Abs())
}

```

对非结构体也可以申明方法

同理，也具有指针类型接收者，例如v *Vertex，此时，就可以在方法中修改值

<mark>：带指针参数的函数必须接受一个指针，而接收者为指针的方法被调用时，接收者即能是指针，又能是值，反之一样</mark>





###### 2.接口（interface）

<mark>：作为一组方法签名</mark>

```go
package main

import (
    "fmt"
    "math"
)

type Abser interface {
    Abs() float64
}

func main() {
    var a Abser
    f := MyFloat(-math.Sqrt2)
    v := Vertex{3, 4}

    a = f  // a MyFloat 实现了 Abser
    a = &v // a *Vertex 实现了 Abser
    fmt.Println(a.Abs())
}

type MyFloat float64

func (f MyFloat) Abs() float64 {
    if f < 0 {
        return float64(-f)
    }
    return float64(f)
}

type Vertex struct {
    X, Y float64
}

func (v *Vertex) Abs() float64 {
    return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

```

Realize：接口就是方法的集合，对于其他类型，像是Myfloat，Vertex，就要实现Abser中的方法这样，任何Abser类型的变量都可以储存Myfloat和Vertex类型的值

<mark>:类型通过实现一个接口的所有方法来实现该接口。既然无需专门显式声明，也就没有“implements”关键字</mark>



```go
package main

import "fmt"

type I interface {
    M()
}//接口

type T struct {
    S string
}//T类型结构体

func (t *T) M() {
    if t == nil {
        fmt.Println("<nil>")
        return
    }
    fmt.Println(t.S)
}//此时空指针异常，M（）方法处理

func main() {
    var i I

    var t *T
    i = t
    describe(i)
    i.M()

    i = &T{"hello"}
    describe(i)
    i.M()
}

func describe(i I) {
    fmt.Printf("(%v, %T)\n", i, i)
}

运行结果：
(<nil>, *main.T)
<nil>
(&{hello}, *main.T)
hello
```

<mark>：接口值也是值，可以传递，在内部，可以看作包含值和具体类型的元组（value,type）</mark>

<mark>：接口值调用方法会执行其底层类型的同名方法</mark>

<mark>：保存了nil具体值的接口本身并不为nil</mark>

<mark>：nil接口值既不保存值，也不保存具体类型，为nil接口调用方法会产生运行时错误，因为接口的元组内并未包含能够指明该调用哪个具体方法的类型</mark>

<mark>：interface{}空接口，可以保存任何类型的值，也可以用来处理未知类型的值</mark>

<mark>：%q格式化一个带双引号，转义后的字符串</mark>



```go
package main

import "fmt"

func main() {
    var i interface{} = "hello"

    s := i.(string)
    fmt.Println(s)
  <hello>
    s, ok := i.(string)
    fmt.Println(s, ok)
  <hello true>
    f, ok := i.(float64)
    fmt.Println(f, ok)
  <0  false>
    f = i.(float64) // panic
    fmt.Println(f)
}


```

<mark>：类型断言提供了访问接口值底层具体值的方式</mark>



###### 3.fmt包中最普遍接口之一----Stringer

```go
package main

import "fmt"

type Person struct {
    Name string
    Age  int
}
//使用Stringer中String()方法
func (p Person) String() string {
    return fmt.Sprintf("%v (%v years)", p.Name, p.Age)
}

func main() {
    a := Person{"Arthur Dent", 42}
    z := Person{"Zaphod Beeblebrox", 9001}
    fmt.Println(a, z)
}

//运行结果：Arthur Dent (42 years) Zaphod Beeblebrox (9001 years)
```

<mark>：fmt.Println()自定义类型输出，自动调用Person的String方法实现自定义输出(Stringer接口的妙用)</mark>



###### 4.fmt中，内接接口error类型：（类似于Stringer接口）

```go
package main

import (
    "fmt"
    "time"
)

type MyError struct {
    When time.Time
    What string
}

func (e *MyError) Error() string {
    return fmt.Sprintf("at %v, %s",
        e.When, e.What)
}

func run() error {
    return &MyError{
        time.Now(),
        "it didn't work",
    }
}

func main() {
    if err := run(); err != nil {
        fmt.Println(err)
    }
}
//if发生错误，进入条件语句， fmt.Println(err)自动调用Error方法打印时间戳和错误信息，err变量存储返回的错误
```

<mark>：go语言中处理错误的重要方式，可以自定义错误类型，自定义打印错误发生时间，错误代码等等，简单，好用，强大！</mark>

<mark>：err := errors.New("something went wrong")，error.New创建错误实例，接受字符串为参数，error.New() 在每次调用时都会返回一个新的错误值，即使它们具有相同的错误消息。</mark>

<mark>：if errors.Is(err, someSpecificError) { }，error.Is检测两错误是否相同</mark>

<mark>：error.As()用于将error转换为具体error类型</mark>

```go
type MyError struct {
    Msg string
}

func (e *MyError) Error() string {
    return e.Msg
}

var MoneyNotEnough = &MyError{Msg: "余额不足"}

func pay(money float64) error {
    if money < 10 {
        return MoneyNotEnough//假使这里传出啥HerError啥的，转换不成功，打印“不是此error类型”
    }
    return nil
}

func main() {
    err := pay(5)
    var myError *MyError
    if errors.As(err, &myError) {
        fmt.Println("转换成功")
        fmt.Println(myError.Error())
    } else {
        fmt.Println("不是此error类型")
    }
}
```

另一种错误处理：panic

```go
func main() {
    //使用匿名函数 将需要保护的代码 控制在一定范围
    func() {
        err := pay(5)
        defer func() {
            if err := recover(); err != nil {//recover函数捕获panic，使之程序继续运行，recover函数只在defer调用函数中有用
                log.Printf("panic:%+v \n", err)
            }
        }()
        if err != nil {
            panic("余额不足")//panic触发，defer函数调用
        }
    }()
    fmt.Println("发生panic后 这里的代码依旧会执行")
}
```

###### 5.io包-Reader接口：表示数据流读取端，在Go标准库中，文件，网络连接等都会实现此接口<读取器>

```go
package main

import (
    "fmt"
    "io"
    "strings"
)

func main() {
    r := strings.NewReader("Hello, Reader!")
        //使用 strings.NewReader 创建一个 Reader，它将字符串 "Hello, Reader!" 作为数据源。
    b := make([]byte, 8)//字节切片，容量为八
    for {
        n, err := r.Read(b)//调用Read方法开始读取
        fmt.Printf("n = %v err = %v b = %v\n", n, err, b)
        fmt.Printf("b[:n] = %q\n", b[:n])
        if err == io.EOF {
            break
        }
    }
}
//输出结果：

n = 8 err = <nil> b = [72 101 108 108 111 44 32 82]
b[:n] = "Hello, R"
n = 6 err = <nil> b = [101 97 100 101 114 33 32 82]
b[:n] = "eader!"
n = 0 err = EOF b = [101 97 100 101 114 33 32 82]
b[:n] = ""
```

<mark>：func (T) Read(b []byte) (n int, err error)，接口的Read方法</mark>

读取文件示例：

```go
func readFile(path string) {
    file, err := os.Open(path)//打开path路径的文件
    if err != nil {           //发生错误，不为空，则终止程序，记录错误
        log.Fatal(err)          
    }
    //注意关闭文件流
    defer file.Close()      //关闭文件读取
    buff := make([]byte, 1024)//创建字节切片
    for {
        n, err := file.Read(buff)//读取
        if err != nil {//发生错误
            if err == io.EOF {//读到文件末尾
                fmt.Println("读完了")
                break
            }
            log.Fatal(err)//否则终止，记录错误
        }
        fmt.Printf("读取到的内容:%v \n ", string(buff[:n]))
    }
}
```

****

###### 6.类型参数：func Index[T comparable](s []T, x T) int

```go
package main

import "fmt"

func Index[T comparable](s []T, x T) int {//s,x为满足内置约束comparable的任何类型T的切片
    for i, v := range s {
        // v 和 x 的类型为 T，它拥有 comparable 可比较的约束，
        // 因此我们可以使用 ==。
        if v == x {
            return i
        }
    }
    return -1
}

func main() {
    // Index 可以在整数切片上使用
    si := []int{10, 20, 15, -10}
    fmt.Println(Index(si, 15))

    // Index 也可以在字符串切片上使用
    ss := []string{"foo", "bar", "baz"}
    fmt.Println(Index(ss, "hello"))
}

//运行结果：
    2
    -1
```

<mark>：Index函数适用于任何支持比较的类型。可以使用==和!=运算符，用comparable约束将值和所有切片元素进行比较，直到找到匹配项</mark>

###### 7.类型联合约束

```go
package main

import "fmt"

// StringOrInt 约束，表示 T 可以是 string 或 int
type StringOrInt interface {
    string | int
}

// MyGenericFunc 接受一个 string 或 int 类型的参数
func MyGenericFunc[T StringOrInt](t T) {
    fmt.Println(t)
}

func main() {
    MyGenericFunc("hello")  // 正确
    MyGenericFunc(10)       // 正确
    MyGenericFunc(3.14)     // 错误：类型不满足约束
}
```



###### 8.Go协程

```go
package main

import (
    "fmt"
    "time"
)

func say(s string) {
    for i := 0; i < 5; i++ {
        time.Sleep(100 * time.Millisecond)//此goroutine休眠100ms,输出有间隔便于观察
        fmt.Println(s)
    }
}

func main() {
    go say("world")//go 关键字可以轻松地启动新的 goroutine，从而实现并发执行
    say("hello")
}

//输出结果有两种
```

<mark>：go调度器的行为会导致执行顺序的不确定性</mark>



###### 9.信道：带有类型的管道

**ch<-v：将v发送至信道ch**

**v:-=<-ch从ch接受值并赋予v**

**ch:=make(chan int)信道在使用前必须创建**

**FIFO:先进先出原则**

```go
package main

import "fmt"

func sum(s []int, c chan int) {
    sum := 0
    for _, v := range s {
        sum += v
    }
    c <- sum // 发送 sum 到 c
}

func main() {
    s := []int{7, 2, 8, -9, 4, 0}

    c := make(chan int)
    go sum(s[:len(s)/2], c)//s前部分
    go sum(s[len(s)/2:], c)//s后部分
    x, y := <-c, <-c // 从 c 接收

    fmt.Println(x, y, x+y)
}
//运行结果：
    -5 17 12
```

<mark>：带缓冲的信道--ch:=make(chan int,100)，仅当信道缓冲区域填满后，向起发送数据才会阻塞，缓冲区为空，接收方会阻塞</mark>

    

###### 10.range与close：

```go
package main

import (
    "fmt"
)

func fibonacci(n int, c chan int) {
    x, y := 0, 1
    for i := 0; i < n; i++ {
        c <- x
        x, y = y, x+y
    }
    close(c)//主动关闭信道
}

func main() {
    c := make(chan int, 10)
    go fibonacci(cap(c), c)//go关键字开启goroutine，cap（c）-容量
        //不断从c信道获值
    for i := range c {
        fmt.Println(i)
    }
}


```



###### 11.select语句：

<mark>：一个Go程等待多个通信操作</mark>

```go
package main

import "fmt"

func fibonacci(c, quit chan int) {
    x, y := 0, 1
                                                     //无限循环的for
    for {
        select {
        case c <- x:                         
            x, y = y, x+y
        case <-quit:                         //如果quit接收到值，终止循环
            fmt.Println("quit")
            return
        }
    }
}

func main() {
    c := make(chan int)
    quit := make(chan int)
    go func() {
        for i := 0; i < 10; i++ {
            fmt.Println(<-c)
        }
        quit <- 0
    }()
    fibonacci(c, quit)//开始创建斐波那契数列
}


```

<mark>：`select` 会阻塞到某个分支可以继续执行为止，这时就会执行该分支。当多个分支都准备好时会随机选择一个执行。</mark>

<mark>：当 `select` 中的其它分支都没有准备好时，`default` 分支就会执行。</mark>

```go
package main

import (
    "fmt"
    "time"
)

func main() {
    tick := time.Tick(100 * time.Millisecond)//每隔100ms，发送一个时间值，打印一次
    boom := time.After(500 * time.Millisecond)//500ms发送时间值，后打印
    for {
        select {
        case <-tick:
            fmt.Println("tick.")
        case <-boom:
            fmt.Println("BOOM!")
            return
        default:
            fmt.Println("    .")
            time.Sleep(50 * time.Millisecond)
                   //每隔50ms打印一次，前提是tick与boom都没有准备好
        }
    }
}


```



###### 12.defer:

<mark>:go语言中，一个特殊的关键字，用于延迟调用</mark>

```go
func main() {
    x := 10
    defer func(x int) {
        //打印10 因为x传参时，值为10
        fmt.Println("我后执行：", x)
    }(x)
    x++
    fmt.Println("我先执行：", x)
    deferTest()
    return
}
func deferTest() {
    var a = 1
    //输出1
    defer fmt.Println(a)
    a = 2
    return
}

结果：
我先执行： 11
1
我后执行： 10
```

<mark>：延迟函数的参数在defer语句出现时已经确认</mark>

<mark>：延迟函数按后进先出执行，即先出现的defer最后执行</mark>

<mark>：延迟函数可以操作主函数的具体返回值</mark>

```go
func main() {
    x := deferTest()//调用函数
    fmt.Println(x)//打印
}
func deferTest() (result int) {//命名返回值result，类型为int
    i := 1//初始化为1
    defer func() {//延迟函数，当返回i后执行操作，使得result返回值+1
        result++
    }()
    return i//返回
}
```

<mark>：如果defer执行的函数为nil，那么会在最终调用函数产生panic</mark>

###### 13.time包：

<秒值，毫秒值>

```go
func main() {
    //当前的时间
    t := time.Now()
    //毫秒值
    milli := t.UnixMilli()
    //秒值
    sencond := t.Unix()
    fmt.Printf("毫秒值:%d, 秒值:%d \n", milli, sencond)
}
```

<时间相加相减>

```go
func main() {
    //当前的时间
    t := time.Now()
    fmt.Println(t.Format("2006-01-02 15:04:05"))//format函数将时间格式化，“2006-01-02 15：04：05”作为模板
    after20s := t.Add(20 * time.Second)
    fmt.Println(after20s.Format("2006-01-02 15:04:05"))
    sub := after20s.Sub(t)//sub()函数计算时间差值
    fmt.Printf("相减时间:%v \n", sub)
}
```

<休眠>

```go
func main() {
    //阻塞当前的go程x时间，x<=0时立即 释放
    time.Sleep(2 * time.Second)
    fmt.Println("等2秒打印出来")
}
```

<经过一段时间返回>

```go
func main() {
    ch := make(chan struct{})
    go wait(ch)
    select {
    case <-ch:
        fmt.Println("wait执行成功")
    case <-time.After(2 * time.Second)://由于休眠时间大于2s,执行以下操作
        fmt.Println("wait等待时间已经超过2秒，超时了")
    }
}

func wait(ch chan struct{}) {
    time.Sleep(3 * time.Second)//休眠时间为3s
    ch <- struct{}{}
}
```

###### 14.io包-Write<写入器>，从缓冲区读取数据，并将数据写入目标资源。

：只要实现了Writer(p []byte),那就是一个写入器

```go
func writeFile(content string) {
    file, err := os.Create("./test.txt")// 新建文件
    if err != nil {                     //错误处理
        fmt.Println(err)
        return
    }
    defer file.Close()                  //无论函数如何结束，关闭文件
    _, err = file.Write([]byte(content))//将content字符串转换为字节切片，并写入到文件中。这里使用下划线_来忽略写入的字节数。
    if err != nil {                     //错误处理
        return
    }
}

```



###### 15.io包-Seeker

<mark>：入参：计算新偏移量的起始值 whence， 基于whence的偏移量offset</mark>

<mark>：返回值：基于 whence 和 offset 计算后新的偏移量值，以及可能产生的错误</mark>

```go
type Seeker interface {
    //用于指定下次读取或者写入时的偏移量
    Seek(offset int64, whence int) (int64, error)
}
const (
    SeekStart   = 0 // 基于文件开始位置
    SeekCurrent = 1 // 基于当前偏移量 
    SeekEnd     = 2 // 基于文件结束位置
)
```

| os.O_WRONLY | 只写   |
| ----------- | ---- |
| os.O_CREATE | 创建文件 |
| os.O_RDONLY | 只读   |
| os.O_RDWR   | 读写   |
| os.O_TRUNC  | 清空   |
| os.O_APPEND | 追加   |

```go
func writeFile(content string) {
    // w写 r读 x执行   w  2   r  4   x  1
    //特殊权限位，拥有者位，同组用户位，其余用户位
  file, err := os.OpenFile("./test.txt", os.O_RDWR, 655)
//参数：打开文件；打开模式；权限控制
	if err != nil {      //处理错误信息
		fmt.Println(err)
		return
	}
	defer file.Close()   //确保函数结束时，关闭文件
	_, err = file.Seek(4, io.SeekStart)//用Seek方法将文件指针移动到文件开始的第5个字节（偏移量为4）。io.SeekStart表示从文件开头开始计算偏移量。
	if err != nil {
		log.Fatal(err)
	}
	_, err = file.Write([]byte(content))
	if err != nil {
		log.Fatal(err)
	}
}
```

![](file:///C:/Users/%E6%99%93/Pictures/Saved%20Pictures/QQ20240727-101338.png)

###### 16.文件操作API：

- 根据提供的文件名创建新的文件，返回一个文件对象，默认权限是0666

```go
 func Create(name string) (file *File, err Error)
```

- 根据文件描述符创建相应的文件，返回一个文件对象

```go
func NewFile(fd uintptr, name string) *File
```

- 只读方式打开一个名称为name的文件

```go
 func Open(name string) (file *File, err Error)
```

- 打开名称为name的文件，flag是打开的方式，只读、读写等，perm是权限

```go
 func OpenFile(name string, flag int, perm uint32) (file *File, err Error)
```

- 写入byte类型的信息到文件

```go
func (file *File) Write(b []byte) (n int, err Error)
```

- 在指定位置开始写入byte类型的信息

```go
 func (file *File) WriteAt(b []byte, off int64) (n int, err Error)
```

- 写入string信息到文件

```go
 func (file *File) WriteString(s string) (ret int, err Error)
```

- 读取数据到b中

```go
 func (file *File) Read(b []byte) (n int, err Error)
```

- 从off开始读取数据到b中

```go
 func (file *File) ReadAt(b []byte, off int64) (n int, err Error)
```

- 删除文件名为name的文件

```go
 func Remove(name string) Error
```

###### 17.bufio包:

<mark>：提供缓冲区，读，写都在缓冲区中，降低访问本地磁盘次数，从而提高效率</mark>

* `bufio.Reader`：代表一个带缓冲的读取器，允许从输入流中按行或字节块读取数据。
* `bufio.Writer`：代表一个带缓冲的写入器，允许将数据写入输出流并控制何时执行实际的写入操作。

![76266afe17e808dfffab5b924e0d0146](https://i-blog.csdnimg.cn/blog_migrate/76266afe17e808dfffab5b924e0d0146.png)

```go
package main

import (
	"bufio"
	"fmt"
	"strings"
)

func main() {
	s := strings.NewReader("ABCDEFG")//创建Reader对象s
	str := strings.NewReader("12345")//创建Reader对象str
	br := bufio.NewReader(s)
//使用bufio.NewReader包装了s，创建了一个bufio.Reader对象br，它可以用来读取数据，并且提供了一些额外的读取功能，比如缓冲。
	b, _ := br.ReadString('\n')//从br中读取数据，直到遇到换行符，这里没有，将读取所有数据，忽略错误
	fmt.Println(b)
	br.Reset(str)              //将br读取源重置为str
	b, _ = br.ReadString('\n')
	fmt.Println(b)
}


```

###### 18.ioutil工具：

* ioutil库包含在io目录下，它的主要作用是`作为一个工具包`，里面有一些比较实用的函数
* 比如 `ReadAll(从某个源读取数据)`、`ReadFile（读取文件内容）`、`WriteFile（将数据写入文件）`、`ReadDir（获取目录）`。

```go
func wr() {
   err := ioutil.WriteFile("./yyy.txt", []byte("码神之路"), 0666)//“码神之路”转为字节切片写入文件中，权限设置为可读写无执行
   if err != nil {                                              //错误信息处理
      fmt.Println(err) 
      return
   }
}

func re() {
   content, err := ioutil.ReadFile("./yyy.txt")                 //读取文件内容
   if err != nil {                                              //处理错误信息
      fmt.Println(err)
      return
   }
   fmt.Println(string(content))
}
```

<mark><网路编程><context包>，请移步：[context包 | 码神之路知识体系 (mszlu.com)](https://www.mszlu.com/go/new/base/18/18.html#_1-%E5%88%9B%E5%BB%BA%E6%A0%B9context)</mark>
