2024.7.15---Go指南：

**方法（带接收者参数的函数）**

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





**接口（interface）**

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
































