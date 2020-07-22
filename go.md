---
title: Go
layout: sheet
prism_languages: [go, bash]
weight: -3
tags: [Featured]
category: C-like
updated: 2020-06-21
---
<!-- 'Sarasa Mono SC', Consolas, 'Courier New', monospace -->
## Getting started
{: .-three-column}

### 介绍
{: .-intro}

- [A tour of Go](https://tour.golang.org/welcome/1) _(tour.golang.org)_
- [Go repl](https://repl.it/languages/go) _(repl.it)_
- [Golang wiki](https://github.com/golang/go/wiki/) _(github.com)_

### Hello world
{: .-prime}

#### hello.go
{: .-file}

```go
package main

import "fmt"

func main() {
  message := greetMe("world")
  fmt.Println(message)
}

func greetMe(name string) string {
  return "Hello, " + name + "!"
}
```

执行：
```bash
$ go build
```

尝试在线运行 Go：
* [Go repl](https://repl.it/languages/go)
* [A Tour of Go](https://tour.golang.org/welcome/1)

### 变量

#### 声明变量

```go
var msg string
msg = "Hello"
```

#### 快速声明（推断类型）

```go
msg := "Hello"
```

### 常量

```go
const Phi = 1.618
```

常量可以是字符，字符串，布尔值或数字值。See: [Constants](https://tour.golang.org/basics/15)

## 基本类型
{: .-three-column}

### Strings

```go
str := "Hello"
```

```go
str := `Multiline
string`
```

Strings are of type `string`.

### Numbers

#### Typical types

```go
num := 3          // int
num := 3.         // float64
num := 3 + 4i     // complex128
num := byte('a')  // byte (alias for uint8)
```

#### Other types

```go
var u uint = 7        // uint (unsigned)
var p float32 = 22.7  // 32-bit float
```

### Arrays

```go
// var numbers [5]int
numbers := [...]int{0, 0, 0, 0, 0}
```

数组的长度是固定的。

### Slices

```go
slice := []int{2, 3, 4}
```

```go
slice := []byte("Hello")
```

切片的长度是不固定的。

### 指针

```go
func main () {
  b := *getPointer()
  fmt.Println("Value is", b)
}
```
{: data-line="2"}

```go
func getPointer () (myPointer *int) {
  a := 234
  return &a
}
```
{: data-line="3"}

```go
a := new(int)
*a = 234
```
{: data-line="2"}

指针指向变量的内存位置。Go 具有完全的垃圾回收机制。See: [Pointers](https://tour.golang.org/moretypes/1)

### 类型转换

```go
i := 2
f := float64(i)
u := uint(i)
```

See: [Type conversions](https://tour.golang.org/basics/13)

## 流程控制
{: .-three-column}

### If

```go
if day == "sunday" || day == "saturday" {
  rest()
} else if day == "monday" && isTired() {
  groan()
} else {
  work()
}
```
{: data-line="1,3,5"}

See: [If](https://tour.golang.org/flowcontrol/5)

### 在 if 中插入语句

```go
if _, err := getResult(); err != nil {
  fmt.Println("Uh oh")
}
```
{: data-line="1"}

这会限制 err 的作用域在 if 的范围内。

See: [If with a short statement](https://tour.golang.org/flowcontrol/6)

### Switch

```go
switch day {
  case "sunday":
    // cases don't "fall through" by default!
    fallthrough

  case "saturday":
    rest()

  default:
    work()
}
```

See: [Switch](https://github.com/golang/go/wiki/Switch)

### For

```go
for count := 0; count <= 10; count++ {
  fmt.Println("My counter is at", count)
}
```

See: [For loops](https://tour.golang.org/flowcontrol/1)

### For-Range

```go
entry := []string{"Jack","John","Jones"}
for i, val := range entry {
  fmt.Printf("At position %d, the character %s is present\n", i, val)
}
```

See: [For-Range loops](https://gobyexample.com/range)

## Functions
{: .-three-column}

### Lambdas

```go
myfunc := func() bool {
  return x > 10000
}
```
{: data-line="1"}

函数是一等公民。

### 多返回值函数

```go
a, b := getMessage()
```

```go
func getMessage() (a string, b string) {
  return "Hello", "World"
}
```
{: data-line="2"}

### 为返回值命名

```go
func split(sum int) (x, y int) {
  x = sum * 4 / 9
  y = sum - x
  return
}
```
{: data-line="4"}

通过在签名中定义返回值名称，`return`（无参数）将返回具有这些名称的变量。

See: [Named return values](https://tour.golang.org/basics/7)

## Packages
{: .-three-column}

### Importing

```go
import "fmt"
import "math/rand"
```

```go
import (
  "fmt"        // gives fmt.Println
  "math/rand"  // gives rand.Intn
)
```

两种引入方式等效

See: [Importing](https://tour.golang.org/basics/1)

### 别名

```go
import r "math/rand"
```
{: data-line="1"}

```go
r.Intn()
```

### 导出名称

```go
func Hello () {
  ···
}
```

用大写字母开头视为包的导出内容。

See: [Exported names](https://tour.golang.org/basics/3)

### Packages

```go
package hello
```

每个 Go 文件都从 `package` 开始。

## 并发
{: .-three-column}

### Goroutines

```go
func main() {
  // A "channel"
  ch := make(chan string)

  // Start concurrent routines
  go push("Moe", ch)
  go push("Larry", ch)
  go push("Curly", ch)

  // Read 3 results
  // (Since our goroutines are concurrent,
  // the order isn't guaranteed!)
  fmt.Println(<-ch, <-ch, <-ch)
}
```
{: data-line="3,6,7,8,13"}

```go
func push(name string, ch chan string) {
  msg := "Hey, " + name
  ch <- msg
}
```
{: data-line="3"}
通道是在 goroutine 中使用的并发安全通信对象。

See: [Goroutines](https://tour.golang.org/concurrency/1), [Channels](https://tour.golang.org/concurrency/2)

### Channels 传冲

```go
ch := make(chan int, 2)
ch <- 1
ch <- 2
ch <- 3
// fatal error:
// all goroutines are asleep - deadlock! 死锁！
```
{: data-line="1"}

缓冲通道限制了它可以保留的消息量。

See: [Buffered channels](https://tour.golang.org/concurrency/3)

### 关闭 channels

#### 关闭 channel

```go
ch <- 1
ch <- 2
ch <- 3
close(ch)
```
{: data-line="4"}

#### 持续遍历直到 channel 关闭

```go
for i := range ch {
  ···
}
```
{: data-line="1"}

#### 当 `ok == false` 即关闭

```go
v, ok := <- ch
```

See: [Range and close](https://tour.golang.org/concurrency/4)

### WaitGroup

```go
import "sync"

func main() {
  var wg sync.WaitGroup
  
  for _, item := range itemList {
    // Increment WaitGroup Counter
    wg.Add(1)
    go doOperation(item)
  }
  // Wait for goroutines to finish
  wg.Wait()
  
}
```
{: data-line="1,4,8,12"}

```go
func doOperation(item string) {
  defer wg.Done()
  // do operation on item
  // ...
}
```
{: data-line="2"}

WaitGroup 是等待 goroutine 的集合完成。 主 goroutine 调用`Add`以设置要等待的 goroutine 的数量。 完成后，主 goroutine 调用`wg.Done()`来等待全部退出。

See: [WaitGroup](https://golang.org/pkg/sync/#WaitGroup)

## 错误处理

### Defer

```go
func main() {
  defer fmt.Println("Done")
  fmt.Println("Working...")
}
```
{: data-line="2"}

推迟运行函数，直到所在的函数返回。
参数将立即求值，但直到稍后才运行函数调用。

See: [Defer, panic and recover](https://blog.golang.org/defer-panic-and-recover)

### Deferring functions

```go
func main() {
  defer func() {
    fmt.Println("Done")
  }()
  fmt.Println("Working...")
}
```
{: data-line="2,3,4"}

Lambdas更适合被传入defer。

```go
func main() {
  var d = int64(0)
  defer func(d *int64) {
    fmt.Printf("& %v Unix Sec\n", *d)
  }(&d)
  fmt.Print("Done ")
  d = time.Now().Unix()
}
```
{: data-line="3,4,5"}
延迟函数使用d的当前值，除非我们使用指针在main末尾获取最终值。

## 结构体 Structs
{: .-three-column}

### 声明

```go
type Vertex struct {
  X int
  Y int
}
```
{: data-line="1,2,3,4"}

```go
func main() {
  v := Vertex{1, 2}
  v.X = 4
  fmt.Println(v.X, v.Y)
}
```

See: [Structs](https://tour.golang.org/moretypes/2)

### 实例化

```go
v := Vertex{X: 1, Y: 2}
```

```go
// Field names can be omitted
v := Vertex{1, 2}
```

```go
// Y is implicit
v := Vertex{X: 1}
```

### 结构体的指针

```go
v := &Vertex{1, 2}
v.X = 2
```

`v.X`与`(*v).X`相同。

## 方法

### 定义

```go
type Vertex struct {
  X, Y float64
}
```

```go
func (v Vertex) Abs() float64 {
  return math.Sqrt(v.X * v.X + v.Y * v.Y)
}
```
{: data-line="1"}

```go
v := Vertex{1, 2}
v.Abs()
```

Go中没有“类”的概念，但可以使用这种方式定义“方法”。

See: [Methods](https://tour.golang.org/methods/1)

### 改变

```go
func (v *Vertex) Scale(f float64) {
  v.X = v.X * f
  v.Y = v.Y * f
}
```
{: data-line="1"}

```go
v := Vertex{6, 12}
v.Scale(0.5)
// `v` is updated
```

传入指针（`*Vertex`）来修改结构体内的值。

See: [Pointer receivers](https://tour.golang.org/methods/4)

## References

### Official resources
{: .-intro}

- [A tour of Go](https://tour.golang.org/welcome/1) _(tour.golang.org)_
- [Golang wiki](https://github.com/golang/go/wiki/) _(github.com)_
- [Effective Go](https://golang.org/doc/effective_go.html) _(golang.org)_

### Other links
{: .-intro}

- [Go by Example](https://gobyexample.com/) _(gobyexample.com)_
- [Awesome Go](https://awesome-go.com/) _(awesome-go.com)_
- [JustForFunc Youtube](https://www.youtube.com/channel/UC_BzFbxG2za3bp5NRRRXJSw) _(youtube.com)_
- [Style Guide](https://github.com/golang/go/wiki/CodeReviewComments) _(github.com)_
