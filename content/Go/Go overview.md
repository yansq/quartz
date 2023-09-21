# Go Overview


Go 的源文件以 `.go` 为后缀名存储在计算机中，这些文件名均由小写字母组成，如 `scanner.go` 。如果文件名由多个部分组成，则使用下划线 `_` 对它们进行分隔，如 `scanner_test.go` 。文件名不包含空格或其他特殊字符。

`_` 本身就是一个特殊的标识符，被称为空白标识符。它可以像其他标识符那样用于变量的声明或赋值（任何类型都可以赋值给它），但任何赋给这个标识符的值都将被抛弃，因此这些值不能在后续的代码中使用，也不可以使用这个标识符作为变量对其它变量进行赋值或运算。

```go
var arr = []int{1, 2, 3, 4, 5}

for _, value := range arr {
    fmt.Printf(value)
} 
```

当标识符（包括常量、变量、类型、函数名、结构字段等等）以一个大写字母开头，如：`Printf`，那么使用这种形式的标识符的对象就可以被外部包的代码所使用（客户端程序需要先导入这个包），这被称为导出（像面向对象语言中的 public）；标识符如果以小写字母开头，则对包外是不可见的，但是它们在整个包的内部是可见并且可用的（像面向对象语言中的 private ）。

---

一个函数可以拥有多返回值，返回类型之间需要使用逗号分割，并使用小括号 `()` 将它们括起来:
```go
func functionName (a int, b string) (r1 int, r2 bool) {
    // ...
}
```
这种多返回值一般用于判断某个函数是否执行成功 (true/false) 或与其它返回值一同返回错误消息（详见之后的并行赋值）。

---

干净、可读的代码和简洁性是 Go 追求的主要目标。通过 `gofmt` 来强制实现统一的代码风格。Go 语言中对象的命名也应该是简洁且有意义的。像 Java 和 Python 中那样使用混合着大小写和下划线的冗长的名称会严重降低代码的可读性。**名称不需要指出自己所属的包**，因为在调用的时候会使用包名作为限定符。**返回某个对象的函数或方法的名称一般都是使用名词，没有 `Get...` 之类的字符**，如果是用于修改某个对象，则使用 `SetName()`。有必须要的话可以使用大小写混合的方式，如 `MixedCaps()` 或 `mixedCaps()`，而不是使用下划线来分割多个名称。

---

`iota` 是一个预定义的标识符，用于枚举常量，初始值是0。每当 `iota` 在新的一行被使用时，它的值都会自动加 1，并且没有赋值的常量默认会应用上一行的赋值表达式:
```go
const (
	a = iota  // a = 0
	b         // b = 1
	c         // c = 2
	d = 5     // d = 5   
	e         // e = 5
)
```

---

`init()`函数会在包完成初始化后自动执行，执行优先级比`main()`函数高。

---

字符串拼接：
- `+`
- `strings.Join()`
- `bytes.Buffer`

```go
var buffer bytes.Buffer
for {
    //method getNextString() not shown here
	if s, ok := getNextString(); ok { 
		buffer.WriteString(s)
	} else {
		break
	}
}
fmt.Println(buffer.String())
```

---

时间格式化
1月2号下午3点4分过5秒，2006年，-7时区
```go
cstZone := time.FixedZone("CST", 8*3600)
t := time.Now().In(cstZone)
// 21 Jun 2023 10:45 +08
fmt.Println(t.Format("02 Jan 2006 15:04 -07"))
```

---

指针
```go
a := 1
b := &a
// 0xc0000a4010
fmt.Println(&a)
// 0xc0000a4010
fmt.Println(b)
// 1
fmt.Println(*b)
```

指针的一个高级应用是你可以传递一个变量的引用（如函数的参数），这样不会传递变量的拷贝。指针传递是很廉价的，只占用 4 个或 8 个字节。当程序在工作中需要占用大量的内存，或很多变量，或者两者都有，使用指针会减少内存占用和提高效率。被指向的变量也保存在内存中，直到没有任何指针指向它们，所以从它们被创建开始就具有相互独立的生命周期。

---
Prefer:

```go
// 代码更清晰，更加容易读懂
func getX2AndX3_2(input int) (x2 int, x3 int) {
    x2 = 2 * input
    x3 = 3 * input
    return
}

// or 
func getX2AndX3_3(input int) (int, int) {
    x2 := 2 * input
    x3 := 3 * input
    return x2, x3
}
```

Rather than:

```go
func getX2AndX3(input int) (int, int) {
    return 2 * input, 3 * input
}
```

---

## defer

Like `finally` in Java, run after the return
```go
func main() {
    // Output:
    // aaa
    // ccc
    // bbb
	function1()
}

func function1() {
	fmt.Printf("aaa\n")
	defer function2()
	fmt.Printf("ccc\n")
}

func function2() {
	fmt.Printf("bbb\n")
}
```

```go
func func1(s string) (n int, err error) {
	defer func() {
		log.Printf("func1(%q) = %d, %v", s, n, err)
	}()
	return 7, io.EOF
}

func main() {
    // Output: 2011/10/04 10:46:11 func1("Go") = 7, EOF
	func1("Go")
}
```

## 闭包

```go

func main() {
    file := MakeAddSuffix(".bmp")("file")
    // file.bmp
    fmt.Printf(file)
}

func MakeAddSuffix(suffix string) func(string) string {
    return func(name string) string {
        if !strings.HasSuffix(name, suffix) {
            return name + suffix
        }
        return name
    }
}
```

## Slice

切片 (slice) 是对数组一个连续片段的**引用**。
使用切片作为函数的传入参数，减少拷贝，降低内存占用。

切片的初始化格式是：`var slice1 []type = arr1[start:end]`。

```go
func sum(a []int) int {
	s := 0
	for _, v := range a {
    	s += v
	}
	return s
}

func main() {
	var arr = [5]int{0, 1, 2, 3, 4}
	// array to slice
	sum(arr[:])
}
```

Using `make` to create a slice when the array has not inited:

```go
// 5: slice length; 10: array capacity
var slice := make([]int, 5, 10)
```

---

```go
s := "hello"
c := []byte(s)
c[0] = 'c'
s2 := string(c) // s2 == "cello"
```

## map

Map doesn't have locking mechanism so it is not thread-safe!

```go
map1 := make(map[string]float32, 10)
map1["key1"] = 4.5
map1["key2"] = 3.14
fmt.Printf(map1["key1"])

// init map
noteFrequency := map[string]float32 {
	"C0": 16.35, "D0": 18.35, "E0": 20.60, "F0": 21.83,
	"G0": 24.50, "A0": 27.50, "B0": 30.87, "A4": 440}
```

Check if a key is in the map: 
```go
_, ok := map1[key1]

// Or with if 
if _, ok := map1[key1]; ok {
    // ...
}
```

Delete key-value from the map:
```go
// if doesn't contains key1, will not throw an error
delete(map1, key1)
```


Get a slice with map type:
```go
package main
import "fmt"

func main() {
	// Version A:
	items := make([]map[int]int, 5)
	for i := range items {
		items[i] = make(map[int]int, 1)
		items[i][1] = 2
	}
	fmt.Printf("Version A: Value of items: %v\n", items)

	// Version B: NOT GOOD!
	items2 := make([]map[int]int, 5)
	for _, item := range items2 {
		item = make(map[int]int, 1) // item is only a copy of the slice element.
		item[1] = 2 // This 'item' will be lost on the next iteration.
	}
	fmt.Printf("Version B: Value of items: %v\n", items2)
}
```

## type

```go
type struct1 struct {
    i1 int
    f1 float42
    str string
}

func main() {
    ms := new(struct1)
    ms.i1 = 10
    ms.f1 = 15.5
    ms.str = "Chris"
    // Or
    ms2 := &struct1{10, 15.5, "Chris"}
    // Or
    var ms3 struct1
    ms3 = struct1{10, 15.5, "Chris"}

    // &{10 15.5 Chris}
    fmt.Println(ms)
}
```

Extends in Go:
```go
type A struct {
    ax, ay int
}

func (p *A) Sum() int {
    return p.ax + pa.ay
} 

type B struct {
    A
    bx, by float32
}
func main() {
    b := B{A{1, 2}, 3.0, 4.0}
    // function Sum() promotes from type A to type B
    b.Sum()
}
```

## Factory

Names of factories start with `new` or `New`

```go
type File struct {
    fd int
    name string
}

func NewFile(fd int, name string) *File {
    if fd < 0 {
        return nil
    }
    return &File(fd, name)
}
```

## method

`method` is a kind of `function`. It has an extra receiver config, like  `this` in Java or other OOP languages.

```go
type TwoInts struct {
    a int
    b int
}

func main() {
    two1 := &TwoInts{12, 10}
    fmt.Printf(two1.AddThem(5))
}

// method
func (tn *TwoInts) AddThem(param int) int {
    return tn.a + tn.b + param
}
```

> [!note]
> 函数将变量作为参数：`Function1(recv)`
> 方法在变量上被调用：`recv.Method1()`

鉴于性能的原因，`recv` 最常见的是一个指向 `receiver_type` 的指针.
如果想要方法改变接收者的数据，就在接收者的指针类型上定义该方法；否则，就在普通的值类型上定义方法。

## interface

```go
type Namer interface {
    Method1(param_list) return_type
    Method2(param_list) return_type
}
```

只包含一个方法的接口的名字由方法名加 `er` 后缀组成，例如 `Printer`、`Reader`、`Writer`、`Logger`、`Converter` 等等。还有一些不常用的方式（当后缀 `er` 不合适时），比如 `Recoverable`，此时接口名以 `able` 结尾，或者以 `I` 开头。

Go 不需要显式地声明类型是否满足某个接口。

```go
type Shaper interface {
    Area() float32
}

type Square struct {
    side float32
}

func (sq *Square) Area() float32 {
    return sq.side * sq.side
}

func main() {
    sq1 := new(Square)
    sq1.side = 5

    var areaIntf Shaper    
    areaIntf = sq1
    // or areaInft := Shaper(sq1)
    // or even areaInft := sq1
    
    fmt.Printf("The square has area: %f\n", areaIntf.Area())
}
```

类型断言，检测接口变量的具体类型：
```go
if v, ok := varI.(T); ok {
    Process(v)
    return
}
```

或是使用 type-switch:
```go
switch t := areaIntf.(type) {
case *Square: 
    fmt.Printf("Type Square %T with value %v\n", t, t)
case *Circle: 
    fmt.Printf("Type Circle %T with value %v\n", t, t)
case nil: 
    fmt.Printf("nil value: nothing to check?\n")
default: 
    fmt.Printf("Unexpected type %T\n", t)
}
```

## 读取用户输入

```go
// 从控制台读取输入:
package main
import "fmt"

var (
   firstName, lastName, s string
   i int
   f float32
   input = "56.12 / 5212 / Go"
   format = "%f / %d / %s"
)

func main() {
   fmt.Println("Please enter your full name: ")
   fmt.Scanln(&firstName, &lastName)
   // fmt.Scanf("%s %s", &firstName, &lastName)
   fmt.Printf("Hi %s %s!\n", firstName, lastName) // Hi Chris Naegels
   fmt.Sscanf(input, format, &f, &i, &s)
   fmt.Println("From the string we read: ", f, i, s)
    // 输出结果: From the string we read: 56.12 5212 Go
}
```

## panic & recover

```go
// panic_recover.go
package main

import (
	"fmt"
)

func badCall() {
	panic("bad end")
}

func test() {
	defer func() {
		if e := recover(); e != nil {
			fmt.Printf("Panicing %s\r\n", e)
		}
	}()
	badCall()
	fmt.Printf("After bad call\r\n") // <-- would not reach
}

// Calling test
// Panicing bad end
// Test completed
func main() {
	fmt.Printf("Calling test\r\n")
	test()
	fmt.Printf("Test completed\r\n")
}
```

