---
layout: post
title:  "Golang基础之第16章-错误处理"
categories: Golang
tags: Golang
---

* content
{:toc}
# 错误处理

## 1.1 什么是错误

错误是什么?

错误指出程序中的异常情况。假设我们正在尝试打开一个文件，文件系统中不存在这个文件。这是一个异常情况，它表示为一个错误。

Go中的错误也是一种类型。错误用内置的`error` 类型表示。就像其他类型的，如int，浮动64，。错误值可以存储在变量中，从函数中返回，等等。

## 1.2 演示错误

让我们从一个示例程序开始，这个程序尝试打开一个不存在的文件。



示例代码：

```go
package main

import (  
    "fmt"
    "os"
)

func main() {  
    f, err := os.Open("/test.txt")
    if err != nil {
        fmt.Println(err)
        return
    }
    fmt.Println(f.Name(), "opened successfully")
}
```

> 在os包中有打开文件的功能函数：
>
> 	func Open(name string) (file \*File, err error)
>
> 如果文件已经成功打开，那么Open函数将返回文件处理。如果在打开文件时出现错误，将返回一个非nil错误。

	

如果一个函数或方法返回一个错误，那么按照惯例，它必须是函数返回的最后一个值。因此，`Open` 函数返回的值是最后一个值。

处理错误的惯用方法是将返回的错误与nil进行比较。nil值表示没有发生错误，而非nil值表示出现错误。在我们的例子中，我们检查错误是否为nil。如果它不是nil，我们只需打印错误并从主函数返回。

运行结果：

```
open /test.txt: No such file or directory
```

我们得到一个错误，说明该文件不存在。

## 1.3 错误类型表示

Go 语言通过内置的错误接口提供了非常简单的错误处理机制。

让我们再深入一点，看看如何定义错误类型的构建。错误是一个带有以下定义的接口类型，

```go
type error interface {
    Error() string
}
```

它包含一个带有Error（）字符串的方法。任何实现这个接口的类型都可以作为一个错误使用。这个方法提供了对错误的描述。

当打印错误时，fmt.Println函数在内部调用Error() 方法来获取错误的描述。这就是错误描述是如何在一行中打印出来的。

**从错误中提取更多信息的不同方法**

既然我们知道错误是一种接口类型，那么让我们看看如何提取更多关于错误的信息。

在上面的例子中，我们仅仅是打印了错误的描述。如果我们想要的是导致错误的文件的实际路径。一种可能的方法是解析错误字符串。这是我们程序的输出，

```
open /test.txt: No such file or directory  
```

我们可以解析这个错误消息并从中获取文件路径"/test.txt"。但这是一个糟糕的方法。在新版本的语言中，错误描述可以随时更改，我们的代码将会中断。

是否有办法可靠地获取文件名？答案是肯定的，它可以做到，标准Go库使用不同的方式提供更多关于错误的信息。让我们一看一看。

 1.断言底层结构类型并从结构字段获取更多信息

如果仔细阅读打开函数的文档，可以看到它返回的是PathError类型的错误。PathError是一个struct类型，它在标准库中的实现如下，

```go
type PathError struct {  
    Op   string
    Path string
    Err  error
}

func (e *PathError) Error() string { return e.Op + " " + e.Path + ": " + e.Err.Error() }  
```

从上面的代码中，您可以理解PathError通过声明错误（）string方法实现了错误接口。该方法连接操作、路径和实际错误并返回它。这样我们就得到了错误信息，

```
open /test.txt: No such file or directory 
```

PathError结构的路径字段包含导致错误的文件的路径。让我们修改上面写的程序，并打印出路径。

修改代码：

```go
package main

import (  
    "fmt"
    "os"
)

func main() {  
    f, err := os.Open("/test.txt")
    if err, ok := err.(*os.PathError); ok {
        fmt.Println("File at path", err.Path, "failed to open")
        return
    }
    fmt.Println(f.Name(), "opened successfully")
}
```

在上面的程序中，我们使用类型断言获得错误接口的基本值。然后我们用错误来打印路径.这个程序输出,

```
File at path /test.txt failed to open  
```

2. 断言底层结构类型，并使用方法获取更多信息

获得更多信息的第二种方法是断言底层类型，并通过调用struct类型的方法获取更多信息。

示例代码：

```go
type DNSError struct {  
    ...
}

func (e *DNSError) Error() string {  
    ...
}
func (e *DNSError) Timeout() bool {  
    ... 
}
func (e *DNSError) Temporary() bool {  
    ... 
}
```

从上面的代码中可以看到，DNSError struct有两个方法Timeout() bool和Temporary() bool，它们返回一个布尔值，表示错误是由于超时还是临时的。

让我们编写一个断言*DNSError类型的程序，并调用这些方法来确定错误是临时的还是超时的。

```go
package main

import (  
    "fmt"
    "net"
)

func main() {  
    addr, err := net.LookupHost("golangbot123.com")
    if err, ok := err.(*net.DNSError); ok {
        if err.Timeout() {
            fmt.Println("operation timed out")
        } else if err.Temporary() {
            fmt.Println("temporary error")
        } else {
            fmt.Println("generic error: ", err)
        }
        return
    }
    fmt.Println(addr)
}
```

在上面的程序中，我们正在尝试获取一个无效域名的ip地址，这是一个无效的域名。golangbot123.com。我们通过声明它来输入*net.DNSError来获得错误的潜在价值。

在我们的例子中，错误既不是暂时的，也不是由于超时，因此程序会打印出来，

```
generic error:  lookup golangbot123.com: no such host  
```

如果错误是临时的或超时的，那么相应的If语句就会执行，我们可以适当地处理它。

3。直接比较

获得更多关于错误的详细信息的第三种方法是直接与类型错误的变量进行比较。让我们通过一个例子来理解这个问题。

filepath包的Glob函数用于返回与模式匹配的所有文件的名称。当模式出现错误时，该函数将返回一个错误ErrBadPattern。

在filepath包中定义了ErrBadPattern，如下所述：

```go
var ErrBadPattern = errors.New("syntax error in pattern")  
```

errors.New()用于创建新的错误。

当模式出现错误时，由Glob函数返回ErrBadPattern。

让我们写一个小程序来检查这个错误：

```go
package main

import (  
    "fmt"
    "path/filepath"
)

func main() {  
    files, error := filepath.Glob("[")
    if error != nil && error == filepath.ErrBadPattern {
        fmt.Println(error)
        return
    }
    fmt.Println("matched files", files)
}
```

运行结果：

```
syntax error in pattern  
```

**不要忽略错误**

永远不要忽略一个错误。忽视错误会招致麻烦。让我重新编写一个示例，该示例列出了与模式匹配的所有文件的名称，而忽略了错误处理代码。

```go
package main

import (  
    "fmt"
    "path/filepath"
)

func main() {  
    files, _ := filepath.Glob("[")
    fmt.Println("matched files", files)
}
```

我们从前面的例子中已经知道模式是无效的。我忽略了Glob函数返回的错误，方法是使用行号中的空白标识符。

```
matched files []  
```

由于我们忽略了这个错误，输出看起来好像没有文件匹配这个模式，但是实际上这个模式本身是畸形的。所以不要忽略错误。



## 1.4 自定义错误

创建自定义错误的最简单方法是使用错误包的新功能。

在使用新函数创建自定义错误之前，让我们了解它是如何实现的。下面提供了错误包中的新功能的实现。

```go
// Package errors implements functions to manipulate errors.
  package errors

  // New returns an error that formats as the given text.
  func New(text string) error {
      return &errorString{text}
  }

  // errorString is a trivial implementation of error.
  type errorString struct {
      s string
  }

  func (e *errorString) Error() string {
      return e.s
  }
```

既然我们知道了新函数是如何工作的，那么就让我们在自己的程序中使用它来创建一个自定义错误。

我们将创建一个简单的程序，计算一个圆的面积，如果半径为负，将返回一个错误。

```go
package main

import (  
    "errors"
    "fmt"
    "math"
)

func circleArea(radius float64) (float64, error) {  
    if radius < 0 {
        return 0, errors.New("Area calculation failed, radius is less than zero")
    }
    return math.Pi * radius * radius, nil
}

func main() {  
    radius := -20.0
    area, err := circleArea(radius)
    if err != nil {
        fmt.Println(err)
        return
    }
    fmt.Printf("Area of circle %0.2f", area)
}
```

运行结果：

```
Area calculation failed, radius is less than zero 
```

使用Errorf向错误添加更多信息

上面的程序运行得很好，但是如果我们打印出导致错误的实际半径，那就不好了。这就是fmt包的Errorf函数的用武之地。这个函数根据一个格式说明器格式化错误，并返回一个字符串作为值来满足错误。

使用Errorf函数，修改程序。

```go
package main

import (  
    "fmt"
    "math"
)

func circleArea(radius float64) (float64, error) {  
    if radius < 0 {
        return 0, fmt.Errorf("Area calculation failed, radius %0.2f is less than zero", radius)
    }
    return math.Pi * radius * radius, nil
}

func main() {  
    radius := -20.0
    area, err := circleArea(radius)
    if err != nil {
        fmt.Println(err)
        return
    }
    fmt.Printf("Area of circle %0.2f", area)
}
```

运行结果：

```
Area calculation failed, radius -20.00 is less than zero  
```

使用struct类型和字段提供关于错误的更多信息

还可以使用将错误接口实现为错误的struct类型。这给我们提供了更多的错误处理的灵活性。在我们的示例中，如果我们想要访问导致错误的半径，那么现在唯一的方法是解析错误描述区域计算失败，半径-20.00小于零。这不是一种正确的方法，因为如果描述发生了变化，我们的代码就会中断。

我们将使用在前面的教程中解释的标准库的策略，在“断言底层结构类型并从struct字段获取更多信息”，并使用struct字段来提供对导致错误的半径的访问。我们将创建一个实现错误接口的struct类型，并使用它的字段来提供关于错误的更多信息。

第一步是创建一个struct类型来表示错误。错误类型的命名约定是，名称应该以文本Error结束。让我们把struct类型命名为areaError

```go
type areaError struct {  
    err    string
    radius float64
}
```

上面的struct类型有一个字段半径，它存储了为错误负责的半径的值，并且错误字段存储了实际的错误消息。

下一步，是实现error 接口

```go
func (e *areaError) Error() string {  
    return fmt.Sprintf("radius %0.2f: %s", e.radius, e.err)
}
```

在上面的代码片段中，我们使用一个指针接收器区域错误来实现错误接口的Error() string方法。这个方法打印出半径和错误描述。

```go
package main

import (  
    "fmt"
    "math"
)

type areaError struct {  
    err    string
    radius float64
}

func (e *areaError) Error() string {  
    return fmt.Sprintf("radius %0.2f: %s", e.radius, e.err)
}

func circleArea(radius float64) (float64, error) {  
    if radius < 0 {
        return 0, &areaError{"radius is negative", radius}
    }
    return math.Pi * radius * radius, nil
}

func main() {  
    radius := -20.0
    area, err := circleArea(radius)
    if err != nil {
        if err, ok := err.(*areaError); ok {
            fmt.Printf("Radius %0.2f is less than zero", err.radius)
            return
        }
        fmt.Println(err)
        return
    }
    fmt.Printf("Area of rectangle1 %0.2f", area)
}
```

程序输出：

```
Radius -20.00 is less than zero
```

使用结构类型的方法提供关于错误的更多信息

在本节中，我们将编写一个程序来计算矩形的面积。如果长度或宽度小于0，这个程序将输出一个错误。

第一步是创建一个结构来表示错误。

```go
type areaError struct {  
    err    string //error description
    length float64 //length which caused the error
    width  float64 //width which caused the error
}
```

上面的错误结构类型包含一个错误描述字段，以及导致错误的长度和宽度。

现在我们有了错误类型，让我们实现错误接口，并在错误类型上添加一些方法来提供关于错误的更多信息。

```go
func (e *areaError) Error() string {  
    return e.err
}

func (e *areaError) lengthNegative() bool {  
    return e.length < 0
}

func (e *areaError) widthNegative() bool {  
    return e.width < 0
}
```

在上面的代码片段中，我们返回`Error() string` 方法的错误描述。当长度小于0时，lengthNegative() bool方法返回true;当宽度小于0时，widthNegative() bool方法返回true。这两种方法提供了更多关于误差的信息，在这种情况下，他们说面积计算是否失败，因为长度是负的，还是宽度为负的。因此，我们使用了struct错误类型的方法来提供更多关于错误的信息。

下一步是写出面积计算函数。

```go
func rectArea(length, width float64) (float64, error) {  
    err := ""
    if length < 0 {
        err += "length is less than zero"
    }
    if width < 0 {
        if err == "" {
            err = "width is less than zero"
        } else {
            err += ", width is less than zero"
        }
    }
    if err != "" {
        return 0, &areaError{err, length, width}
    }
    return length * width, nil
}
```

上面的rectArea函数检查长度或宽度是否小于0，如果它返回一个错误消息，则返回矩形的面积为nil。

主函数：

```go
func main() {  
    length, width := -5.0, -9.0
    area, err := rectArea(length, width)
    if err != nil {
        if err, ok := err.(*areaError); ok {
            if err.lengthNegative() {
                fmt.Printf("error: length %0.2f is less than zero\n", err.length)

            }
            if err.widthNegative() {
                fmt.Printf("error: width %0.2f is less than zero\n", err.width)

            }
            return
        }
        fmt.Println(err)
        return
    }
    fmt.Println("area of rect", area)
}
```

运行结果：

```
error: length -5.00 is less than zero  
error: width -9.00 is less than zero 
```



其他案例：

函数通常在最后的返回值中返回错误信息。使用errors.New 可返回一个错误信息

```go
func Sqrt(f float64) (float64, error) {
    if f < 0 {
        return 0, errors.New("math: square root of negative number")
    }
    // 实现
}
```

```go
package main

import (
	"fmt"
)

// 定义一个 DivideError 结构
type DivideError struct {
	dividee int
	divider int
}

// 实现 	`error` 接口
func (de *DivideError) Error() string {
	strFormat := `
	Cannot proceed, the divider is zero.
	dividee: %d
	divider: 0
`
	return fmt.Sprintf(strFormat, de.dividee)
}

// 定义 `int` 类型除法运算的函数
func Divide(varDividee int, varDivider int) (result int, errorMsg string) {
	if varDivider == 0 {
		dData := DivideError{
			dividee: varDividee,
			divider: varDivider,
		}
		errorMsg = dData.Error()
		return
	} else {
		return varDividee / varDivider, ""
	}

}

func main() {

	// 正常情况
	if result, errorMsg := Divide(100, 10); errorMsg == "" {
		fmt.Println("100/10 = ", result)
	}
	// 当被除数为零的时候会返回错误信息
	if _, errorMsg := Divide(100, 0); errorMsg != "" {
		fmt.Println("errorMsg is: ", errorMsg)
	}

}
```

`结果`

```go
100/10 =  10
errorMsg is:  
	Cannot proceed, the divider is zero.
	dividee: 100
	divider: 0
```














