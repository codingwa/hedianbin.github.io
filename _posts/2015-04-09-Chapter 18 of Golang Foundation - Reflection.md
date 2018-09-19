---
layout: post
title:  "Golang基础之第18章-反射"
categories: Golang
tags: Golang
---

* content
{:toc}
# 反射reflect

## 一、引入

Java的反射机制是其标志性的特征之一，正是这种语言本身支持的强大的机制使得很多流行的框架有了用武之地。C++中虽然也能实现，但是语言本身并没有提供标准的支持。 
而作为一门现代的语言，go语言也引入了反射机制，在这篇文章中我们将会了解一下go语言中的反射机制是如何使用的。

反射机制
反射机制是程序能够检查其自身结构，属于元编程的范畴，强大的同时也往往是困扰的源头。虽然各种语言的反射模型有所不同，但是通过简单的比较也能有所收获。在了解Go的反射机制之前先来看看Java的反射机制吧。

Java的反射机制
我们所熟知的Java的反射机制是什么？对于类和对象的使用，普通的方式是知道类和对象的属性和方法之后进行调用或者访问。 
而反射机制，简单来说，是在运行状态中，Java对于任何的类，都能够确认到这个类的所有方法和属性；对于任何一个对象，都能调用它的任意方法和属性。这种动态获取或者调用的方式就是Java的反射机制。



能做什么
在Java中，通过反射机制在运行时能够做到如下：

确认对象的类
确认类的所有成员变量和方法
动态调用任意一个对象的方法
…

## 二、相关基础

在进行更加详细的了解之前，我们需要重新温习一下Go语言相关的一些特性，所谓温故知新，从这些特性中了解其反射机制是如何使用的。

| 特点                   | 说明                                                         |
| ---------------------- | ------------------------------------------------------------ |
| go语言是静态类型语言。 | 编译时类型已经确定，比如对已基本数据类型的再定义后的类型，反射时候需要确认返回的是何种类型。 |
|空接口interface{}|go的反射机制是要通过接口来进行的，而类似于Java的Object的空接口可以和任何类型进行交互，因此对基本数据类型等的反射也直接利用了这一特点|
## 三、反射的使用

所谓反射就是动态运行时的状态。我们一般用到的包是reflect包

使用reflect一般分成三步：

首先需要把它转化成reflect对象(reflect.Type或者reflect.Value，根据不同的情况调用不同的函数)

```go
t := reflect.TypeOf(i) //得到类型的元数据,通过t我们能获取类型定义里面的所有元素
v := reflect.ValueOf(i) //得到实际的值，通过v我们获取存储在里面的值，还可以去改变值
```

获取反射值能返回相应的类型和数值

```go
var x float64 = 3.4
v := reflect.ValueOf(x)
fmt.Println("type:", v.Type())
fmt.Println("kind is float64:", v.Kind() == reflect.Float64)
fmt.Println("value:", v.Float())
```

如果是struct的话，可以使用Elem()

```go
tag := t.Elem().Field(0).Tag //获取定义在struct里面的Tag属性
name := v.Elem().Field(0).String() //获取存储在第一个字段里面的值
```

修改

```go
var x float64 = 3.4
p := reflect.ValueOf(&x)
v := p.Elem()//必须的步骤
v.SetFloat(7.1)
```
示例代码

```go
package main

import (
	"reflect"
	"fmt"
)

func main()  {
	//1.“接口类型变量”=>“反射类型对象”
	var circle float64 = 6.28
	var icir interface{}

	icir = circle
	fmt.Println("Reflect : circle.Value = ", reflect.ValueOf(icir)) //Reflect : circle.Value =  6.28
	fmt.Println("Reflect : circle.Type  = ", reflect.TypeOf(icir)) //Reflect : circle.Type =  float64

	// 2. “反射类型对象”=>“接口类型变量
	v1 := reflect.ValueOf(icir)
	fmt.Println(v1) //6.28
	fmt.Println(v1.Interface()) //6.28

	y := v1.Interface().(float64)
	fmt.Println(y) //6.28

	//v1.SetFloat(4.13) //panic: reflect: reflect.Value.SetFloat using unaddressable value
	//fmt.Println(v1)

	//3.修改
	fmt.Println(v1.CanSet())//是否可以进行修改
	v2 := reflect.ValueOf(&circle) // 传递指针才能修改
	v4:=v2.Elem()// 传递指针才能修改,获取Elem()才能修改
	fmt.Println(v4.CanSet()) //true
	v4.SetFloat(3.14)
	fmt.Println(circle) //3.14

}

```

## 四、结构体

### 4.1可以通过反射，获取结构体对象的属性和方法

### 4.2可以通过反射，调用结构体方法

示例代码：

```go
package main

import (
	"fmt"
	"reflect"
)

//1.提供一个结构体
type Person struct {
	Name string
	Age int
	Sex string
}
//2.提供一个方法
func (p Person) Say(msg string)  {
	fmt.Println("Hello..", msg)
}

func (p Person) PrintInfo()  {
	fmt.Println("姓名：",p.Name,"年龄：",p.Age,"性别：",p.Sex)
}

func main()  {
	p1:=Person{"王二狗",30,"男"}
	//反射使用 TypeOf 和 ValueOf 函数从接口中获取目标对象信息
	//1.获取对象的类型
	t1:=reflect.TypeOf(p1)
	fmt.Println(t1) //main.Person
	fmt.Println("p1的类型是：",t1.Name())//调用t.Name方法来获取这个类型的名称
	k1:=t1.Kind() //struct
	fmt.Println(k1)
	//2.获取值，如果是结构体类型，获取的是字段的值
	v1:=reflect.ValueOf(p1) //{王二狗 30 男}
	fmt.Println(v1)
	if t1.Kind() == reflect.Struct{
		//是结构体类型，获取里面的字段名字
		fmt.Println(t1.NumField()); //3
		for i:=0;i<t1.NumField();i++{
			field := t1.Field(i)
			//fmt.Println(field) //{Name  string  0 [0] false},{Age  int  16 [1] false},{Sex  string  24 [2] false}
			val:=v1.Field(i).Interface()//通过interface方法来取出这个字段所对应的值
			fmt.Printf("字段名字：%s,字段类型：%s,字段数值：%v\n",field.Name,field.Type,val)
		}
	}


	//2.操作方法
	for i:=0;i<t1.NumMethod();i++{
		m:=t1.Method(i)
		fmt.Println(m.Name,m.Type) //Hello func(main.Person)
		/*
		{Hello  func(main.Person) <func(main.Person) Value> 0}
		{PrintInfo  func(main.Person) <func(main.Person) Value> 1}
		 */
	}

	m1 := v1.MethodByName("Say")
	args:=[]reflect.Value{reflect.ValueOf("干啥呢？")}
	m1.Call(args)

	m2:=v1.MethodByName("PrintInfo")
	m2.Call(nil)

}

```

结构体中包含匿名结构体

```go
package main

import (
	"reflect"
	"fmt"
)

type Animal struct {
	Name string
	Age int
}
type Cat struct {
	Animal
	Color string
}
// 获取匿名字段
func main()  {
	c1:= Cat{Animal{"猫咪",1},"白色"}
	t1:=reflect.TypeOf(c1)

	for i:=0;i<t1.NumField();i++{
		fmt.Println(t1.Field(i))
		/*
		{Animal  main.Animal  0 [0] true}
		{Color  string  24 [1] false}
		 */
	}
	// FiledByIndex()的参数是一个切片，第一个数是Animal字段，第二个参数是Animal的第一个字段
	f1:=t1.FieldByIndex([]int{0,0})
	f2:=t1.FieldByIndex([]int{0,1})
	fmt.Println(f1)//{Name  string  0 [0] false}
	fmt.Println(f2) //{Age  int  16 [1] false}

	v1:=reflect.ValueOf(c1)
	fmt.Println(v1.Field(0)) //{猫咪 1}
	fmt.Println(v1.FieldByIndex([]int{0,0})) //猫咪
}

```



### 4.3.可以通过反射，修改结构体的数据

示例代码：

```go
package main

import (
	"reflect"
	"fmt"
)

type Student struct {
	Name string
	Age int
	School string
}
func main()  {
	/*
	修改内容
	 */
	s1:= Student{"王二狗",18,"清华大学"}
	v1 := reflect.ValueOf(&s1)
	
	if v1.Kind() ==reflect.Ptr && v1.Elem().CanSet(){
		v1 = v1.Elem()
		fmt.Println("可以修改。。")
	}
	f1:=v1.FieldByName("Name")
	fmt.Println(f1.CanSet())
	f1.SetString("王三狗")
	f2:=v1.FieldByName("Age")
	fmt.Println(f2.CanSet())
	f2.SetInt(20)
	fmt.Println(s1)
}
```

