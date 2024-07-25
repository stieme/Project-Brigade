##### 2024.7.15---Go指南：

###### 1.方法（带接收者参数的函数）

```
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

```
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

```
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



```
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



```类型断言
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

```
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

```
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



###### 5.io包中Reader接口：表示数据流读取端，在Go标准库中，文件，网络连接等都会实现此接口

```
package main

import (
    "fmt"
    "io"
    "strings"
)

func main() {
    r := strings.NewReader("Hello, Reader!")
        //使用 strings.NewReader 创建一个 Reader，它将字符串 "Hello, Reader!" 作为数据源。
    b := make([]byte, 8)
    for {
        n, err := r.Read(b)
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

****

###### 6.类型参数：func Index[T comparable](s []T, x T) int

```
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

```
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

```
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

```
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

```
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

```
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

```
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
