---
layout: post
title:  "Golang之缓冲器bytes.Buffer"
categories: Golang
tags: Golang bytes
---

* content
{:toc}
# Golang之缓冲器bytes.Buffer

## 声明一个Buffer的四种方法：
var b bytes.Buffer       //直接定义一个Buffer变量，不用初始化，可以直接使用
b := new(bytes.Buffer)   //使用New返回Buffer变量
b := bytes.NewBuffer(s []byte)   //从一个[]byte切片，构造一个Buffer
b := bytes.NewBufferString(s string)   //从一个string变量，构造一个Buffer

## 往Buffer中写入数据
b.Write(d []byte)        //将切片d写入Buffer尾部
b.WriteString(s string)  //将字符串s写入Buffer尾部
b.WriteByte(c byte)     //将字符c写入Buffer尾部
b.WriteRune(r rune)     //将一个rune类型的数据放到缓冲器的尾部
b.WriteTo(w io.Writer)  //将Buffer中的内容输出到实现了io.Writer接口的可写入对象中

注：将文件中的内容写入Buffer,则使用ReadForm(i io.Reader)

## 从Buffer中读取数据到指定容器
c := make([]byte,8)
b.Read(c)      //一次读取8个byte到c容器中，每次读取新的8个byte覆盖c中原来的内容
b.ReadByte()   //读取第一个byte，b的第1个byte被拿掉，赋值给a => a, _ := b.ReadByte()

b.ReadRune()   //读取第一个rune，b的第1个rune被拿掉，赋值给r => r, _ := b.ReadRune()
b.ReadBytes(delimiter byte)   //需要一个byte作为分隔符，读的时候从缓冲器里找第一个出现的分隔符（delim），找到后，把从缓冲器头部开始到分隔符之间的所有byte进行返回，作为byte类型的slice，返回后，缓冲器也会空掉一部分
b.ReadString(delimiter byte)  //需要一个byte作为分隔符，读的时候从缓冲器里找第一个出现的分隔符（delim），找到后，把从缓冲器头部开始到分隔符之间的所有byte进行返回，作为字符串返回，返回后，缓冲器也会空掉一部分
b.ReadForm(i io.Reader)  //从一个实现io.Reader接口的r，把r里的内容读到缓冲器里，n 返回读的数量
​    file, _ := os.Open("./text.txt")    
​    buf := bytes.NewBufferString("Hello world")    
​    buf.ReadFrom(file)              //将text.txt内容追加到缓冲器的尾部    
​    fmt.Println(buf.String())

## 清空数据
b.Reset()

## 字符串化
b.String()