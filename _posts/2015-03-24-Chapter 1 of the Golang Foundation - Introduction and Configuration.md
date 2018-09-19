---
layout: post
title:  "Golang基础之第1章-简介和配置"
categories: JavaScript
tags: JavaScript Array 算法 慕课网
---

* content
{:toc}

# 一、介绍和安装

## 1.介绍 

创立时间
2007年 google作为20%项目开始研发 
2009年11月10日 开源，获得TIOBE年度语言 
2012年3月28日 发布Go1.0版本 
2016年8月18日 发布Go1.7版本

### 1.1 什么是Golang

Go也被称为Golang，它是由谷歌创建的一种开源、编译和静态类型的编程语言。

Golang的主要目标是使高可用性和可伸缩的web应用程序的开发变得简单易行。

### 1.2 为什么选择Golang

当有很多其他语言(如python、ruby、node.js)时，为什么选择Golang作为服务端编程语言呢?

- 并发是语言的一个固有部分。因此编写多线程程序是小菜一碟。这是通过Goroutines和channel实现的，我们将在后面的教程中讨论。
- Golang是一种编译语言。源代码编译成原生二进制。这在解释语言(如nodejs中使用的JavaScript)中丢失了。
- 语言规范非常简单。整个规范适合于一个页面，您甚至可以使用它来编写自己的编译器:)
- go编译器支持静态链接。所有的go代码都可以静态地链接到一个大的fat二进制文件中，并且可以轻松地部署到云服务器中，而不用担心依赖关系。

## 2.安装

### 2.1下载

在Mac、Windows和Linux三个平台上都支持Golang。您可以从[https://golang.org/dl/](https://golang.org/dl/)下载相应平台的二进制文件。

![](http://om1c35wrq.bkt.clouddn.com/001_%E4%B8%8B%E8%BD%BD%E5%AE%89%E8%A3%85%E5%8C%85.jpg)



Mac OS
从https://golang.org/dl/下载osx安装程序。双击启动安装。按照提示，这应该在/usr/local/go中安装了Golang，并且还会将文件夹/usr/local/go/bin添加到您的PATH环境变量中。

Windows
从https://golang.org/dl/下载MSI安装程序。双击启动安装并遵循提示。这将在位置c中安装Golang:\Go，并且还将添加目录c:\Go\bin到您的path环境变量。

Linux
从https://golang.org/dl/下载tar文件，并将其解压到/usr/local。

将/usr/local/go/bin添加到PATH环境变量中。这应该安装在linux中。



### 2.2 windows下安装并配置环境变量

安装步骤就不在多说什么了，一路到底

**A、配置环境变量**

注意：如果是msi安装文件，Go语言的环境变量会自动设置好。

我的电脑——右键“属性”——“高级系统设置”——“环境变量”——“系统变量”

	假设GO安装于C盘根目录

**新建：**

- GOROOT：Go安装路径（例：C:\Go）

- GOPATH：Go工程的路径（例：E:\go）。如果有多个，就以分号分隔添加

  ![](http://om1c35wrq.bkt.clouddn.com/%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F3.jpg)

**修改：**

- Path：在path中增加：C:\Go\bin;%GOPATH%\bin;

  > 需要把GOPATH中的可执行目录也配置到环境变量中, 否则你自行下载的第三方go工具就无法使用了

  ​

  ![](http://om1c35wrq.bkt.clouddn.com/%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F2.jpg)

> 1. 工作目录就是我们用来存放开发的源代码的地方，对应的也是Go里的GOPATH这个环境变量。这个环境变量指定之后，我们编译源代码等生成的文件都会放到这个目录下，GOPATH环境变量的配置参考上面的安装Go，配置到Windows下的系统变量里。
> 2. GOPATH之下主要包含三个目录: bin、pkg、src。bin目录主要存放可执行文件; pkg目录存放编译好的库文件, 主要是*.a文件; src目录下主要存放go的源文件



**B、查看是否安装配置成功**

使用快捷键win+R键，输入cmd，打开命令行提示符，在命令行中输入

```go
go env  # 查看得到go的配置信息
go version  # 查看go的版本号
```



### 2.3 mac系统安装并配置

**安装**

双击pkg包，顺着指引，即可安装成功。 
在命令行输入 go version，获取到go的版本号，则代表安装成功。

**配置环境变量**

1、打开终端输入cd ~进入用户主目录; 
2、输入ls -all命令查看是否存在.bash_profile; 
3、存在既使用vim .bash_profile 打开文件; 
4、输入 i 进入vim编辑模式； 
5、输入下面代码， 
其中 GOPATH: 日常开发的根目录。GOBIN:是GOPATH下的bin目录。

> `export GOPATH=/Users/ruby/go`
>
> `export GOBIN=$GOPATH/bin`
>
> `export PATH=$PATH:$GOBIN`

6、点击ESC，并输入 :wq 保存并退出编辑。可输入vim .bash_profile 查看是否保存成功。

7、输入source ~/.bash_profile 完成对golang环境变量的配置，配置成功没有提示。 
8、输入go env 查看配置结果





# 二、搭建开发工具

## 2.1 使用atom

安装好atom工具，然后安装go-plus插件和atom-terminal-panel等插件。

1.安装go-plus插件，这个插件提供了Atom中几乎所有go语言开发的支持，包括 tools, build flows, linters, vet 和 coverage tools。它还包含很多代码片段和一些其它特性。

![](http://om1c35wrq.bkt.clouddn.com/22188-96e2f6da75383392.png)

2.language-go

![](http://om1c35wrq.bkt.clouddn.com/language-go.jpg)

3.安装file-icon插件，它提针对不同后缀的文件，提供了大量的icon显示。

![](http://om1c35wrq.bkt.clouddn.com/002_%E6%8F%92%E4%BB%B6.bmp)



4.设置字体大小等

![](http://om1c35wrq.bkt.clouddn.com/%E8%AE%BE%E7%BD%AE%E5%AD%97%E4%BD%93.jpg)



> atom快捷键大全，参照：[https://www.cnblogs.com/hubgit/p/5130192.html](https://www.cnblogs.com/hubgit/p/5130192.html)



## 2.2 使用Goland

下载地址：<http://www.jetbrains.com/go/>

傻瓜式安装，一路next，直到完成。

打开Goland工具

![](http://om1c35wrq.bkt.clouddn.com/land1.bmp)

创建项目：

![](http://om1c35wrq.bkt.clouddn.com/%E5%88%9B%E5%BB%BA%E9%A1%B9%E7%9B%AE.bmp)



常用快捷键：https://blog.csdn.net/benben_2015/article/details/78813670

## 2.3 其他开发工具

比如sublime text，editplus，notpad++，eclipse等等。。



# 三、第一个程序：HelloWorld

### 3.1 编写第一个程序

1.打开编辑器创建一个新的helloworld.go文件，并输入以下内容：

```go
package main

import "fmt"

func main() {
   /* 输出 */
   fmt.Println("Hello, World!")
}
```

2.执行go程序

执行go程序由几种方式

方式一：使用go run命令

	step1：使用快捷键win+R，输入cmd打开命令行提示符
	
	step2：进入helloworld.go所在的目录
	
	step3：输入go run helloworld.go命令并观察运行结果。

方式二：使用go build命令

	step1：使用快捷键win+R，输入cmd打开命令行提示符
	
	step2：进入helloworld.go所在的目录
	
	step3：输入go build helloworld.go命令进行编译，产生同名的helloworld.exe文件
	
	step4：输入helloworld.exe，执行



方式三：使用 go playground 

	step1：打开一下网址https://play.golang.org/


	

### 3.2 第一个程序的解释说明

#### 3.2.1 package

- 在同一个包下面的文件属于同一个工程文件，不用`import`包，可以直接使用
- 在同一个包下面的所有文件的package名，都是一样的
- 在同一个包下面的文件`package`名都建议设为是该目录名，但也可以不是

#### 3.2.2  import

import "fmt" 告诉 Go 编译器这个程序需要使用 fmt 包的函数，fmt 包实现了格式化 IO（输入/输出）的函数

可以是相对路径也可以是绝对路径，推荐使用绝对路径（起始于工程根目录）

1. 点操作
   我们有时候会看到如下的方式导入包

   ```go
   import(
   	. "fmt"
   ) 
   ```

   这个点操作的含义就是这个包导入之后在你调用这个包的函数时，你可以省略前缀的包名，也就是前面你调

   用的`fmt.Println("hello world")`可以省略的写成`Println("hello world")`

2. 别名操作
   别名操作顾名思义我们可以把包命名成另一个我们用起来容易记忆的名字

   ```go
   import(
   	f "fmt"
   ) 
   ```

   别名操作的话调用包函数时前缀变成了我们的前缀，即`f.Println("hello world")`

3. _操作
   这个操作经常是让很多人费解的一个操作符，请看下面这个import

   ```go
   import (
     "database/sql"
     _ "github.com/ziutek/mymysql/godrv"
   ) 
   ```

   _操作其实是引入该包，而不直接使用包里面的函数，而是调用了该包里面的init函数

#### 3.3.3 main

main(),是程序运行的入口。



# 四、编码规范

### 4.1 编码规范

缩进

空格

### 4.2 注释

- 单行注释是最常见的注释形式，你可以在任何地方使用以 // 开头的单行注释
- 多行注释也叫块注释，均已以 /* 开头，并以 */ 结尾，且不可以嵌套使用，多行注释一般用于包的文档描述或注释成块的代码片段

### 4.3 标识符

Go标识符是用来标识变量、函数或任何其他用户定义项的名称。标识符以字母a到Z或a到Z或下划线开头，后面跟着零或更多的字母、下划线和数字(0到9)。Go不允许在标识符中使用@、$和%等标点符号。Go是一种区分大小写的编程语言。因此，Manpower和manpower是两个不同的标识符。

> 1. 当标识符（包括常量、变量、类型、函数名、结构字段等等）以一个大写字母开头，如：Group1，那么使用这种形式的标识符的对象就**可以被外部包的代码所使用**（客户端程序需要先导入这个包），这被称为导出（像面向对象语言中的 public）；
> 2. **标识符如果以小写字母开头，则对包外是不可见的，但是他们在整个包的内部是可见并且可用的**（像面向对象语言中的 private ）



### 4.4 语句的结尾

Go语言中是不需要类似于Java需要冒号结尾，默认一行就是一条数据

如果你打算将多个语句写在同一行，它们则必须使用 **;** 

### 4.5 关键字

下面的列表显示了Go中的保留字。这些保留字不能用作常量或变量或任何其他标识符名称。

![](http://om1c35wrq.bkt.clouddn.com/guanjianzi.jpg)