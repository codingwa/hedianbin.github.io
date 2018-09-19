---
layout: post
title:  "Golang基础之第16章-网络编程"
categories: Golang
tags: Golang
---

* content
{:toc}
# 网络编程

## 一、网络编程基础

go 的网络编程模块主要支持两种Internet协议: TCP 和 UDP.

### 1.1通信协议

通信协议也叫网络传输协议或简称为传送协议（Communications Protocol），是指计算机通信或网络设备的共同语言。

现在最普及的计算机通信为网络通信，所以“传送协议”一般都指计算机通信的传送协议，如：TCP/IP、NetBEUI、HTTP、FTP等。

然而，传送协议也存在于计算机的其他形式通信，例如：面向对象编程里面对象之间的通信；操作系统内不同程序之间的消息，都需要有一个传送协议，以确保传信双方能够沟通无间。



------

### 1.2`TCP/IP`协议

在Internet中`TCP/IP`协议是使用最为广泛的通讯协议(**互联网上的一种事实的标准**)。TCP/IP是英文`Transmission Control Protocol/Internet Protocol`的缩写，意思是“传输控制协议/网际协议”

TCP/IP 协议是一个工业标准协议套件，专为跨广域网(WAN)的大型互联网络而设计。 

TCP/IP 网络体系结构模型就是遵循TCP/IP 协议进行通信的一种分层体系，现今，Internet和Intranet所使用的协议一般都为TCP/IP 协议。

在了解该协议之前，我们必须掌握基于该协议的体系结构层次，而TCP/IP体系结构分为四层。

第 1 层 网络接口层
包括用于协作IP数据在已有网络介质上传输的协议，提供TCP/IP协议的数据结构和实际物理硬件之间的接口。比如地址解析协议(Address Resolution Protocol, ARP )等。

第 2 层 网络层
对应于OSI模型的网络层，主要包含了IP、RIP等相关协议，负责数据的打包、寻址及路由。还包括网间控制报文协议(ICMP)来提供网络诊断信息。

第 3 层 传输层
对应于OSI的传输层，提供了两种端到端的通信服务，分别是TCP和UDP协议。

第 4 层 应用层
对应于OSI的应用层、表达层和会话层，提供了网络与应用之间的对话接口。包含了各种网络应用层协议，比如Http、FTP等应用协议。

![](http://o7cqr8cfk.bkt.clouddn.com/17-7-29/99460206.jpg)

------

#### 附录:OSI 七层参考模型

<img src="http://o7cqr8cfk.bkt.clouddn.com/markdown/1501298857841.png">

------

### 1.3 IP 地址和端口号

#### 1.3.1 IP 地址

互联网协议地址（英语：Internet Protocol Address，又译为网际协议地址），缩写为**IP地址**（英语：IP Address）

IP 地址是分配给网络上使用网际协议（英语：Internet Protocol, IP）的设备的数字标签。常见的IP地址分为IPv4与IPv6两大类。

------

##### IPV4地址

IP地址由32位二进制数组成，为便于使用，常以XXX.XXX.XXX.XXX形式表现，每组XXX代表小于或等于255的10进制数。例如维基媒体的一个IP地址是208.80.152.2。

地址可分为A、B、C、D、E五大类，其中E类属于特殊保留地址。
IP地址是唯一的。目前IP技术可能使用的IP地址最多可有4,294,967,296个（即232）。骤看可能觉得很难会用尽，但由于早期编码和分配上的问题，使很多区域的编码实际上被空出或不能使用。加上互联网的普及，使大部分家庭都至少有一部电脑，连同公司的电脑，以及连接网络的各种设备都消耗了大量IPv4地址资源。

随着互联网的快速成长，IPv4的42亿个地址的分配最终于2011年2月3日用尽[1][2]。相应的科研组织已研究出128位的IPv6，其IP地址数量最高可达3.402823669 × 1038个，届时每个人家居中的每件电器，每件对象，甚至地球上每一粒沙子都可以拥有自己的IP地址。

在A类、B类、C类IP地址中，如果主机号是全1，那么这个地址为直接广播地址，它是用来使路由器将一个分组以广播形式发送给特定网络上的所有主机。32位全为1的IP地址“255.255.255.255”为受限广播地址（"limited broadcast" destination address），用来将一个分组以广播方式发送给本网络中的所有主机，路由器则阻挡该分组通过，将其广播功能限制在本网内部。

------

##### IPV6地址

IPv6地址为128位长但通常写作8组每组四个十六进制数的形式。例如：
2001:0db8:85a3:08d3:1319:8a2e:0370:7344
是一个合法的IPv6地址。

IPv4地址可以很容易的转化为IPv6格式。举例来说，如果IPv4的一个地址为135.75.43.52（十六进制为0x874B2B34），它可以被转化为0000:0000:0000:0000:0000:0000:874B:2B34或者::874B:2B34。同时，还可以使用混合符号（IPv4-compatible address），则地址可以为::135.75.43.52。

GO支持IP的类型

在Go的net包中定义了很多类型、函数和方法用来网络编程，其中IP的定义如下：

```go
type IP []byte
```

在net包中有很多函数来操作IP，但是其中比较有用的也就几个，其中ParseIP(s string) IP函数会把一个IPv4或者IPv6的地址转化成IP类型，请看下面的例子:

```go
name := "192.168.10.105:8282"
addr := net.ParseIP(name)
if addr == nil {
   fmt.Println("Invalid address")
} else {
   fmt.Println("The address is ", addr.String())
}
```

执行之后你就会发现只要你输入一个IP地址就会给出相应的IP格式

------

#### 1.3.2端口

在网络技术中，端口(Port)包括逻辑端口和物理端口两种类型。

物理端口指的是物理存在的端口，如ADSL Modem、集线器、交换机、路由器上用 于连接其他网络设备的接口，如RJ-45端口、SC端口等等。

逻辑端口是指逻辑意义上用于区分服务的端口，如TCP/IP协议中的服务端口，端口号的范围从0到65535，比如用于浏览网页服务的80端口，用于FTP服务的21端口等。由于物理端口和逻辑端口数量较多，为了对端口进行区分，将每个端口进行了编号，这就是端口号。

我们主要研究的是逻辑端口号.我们平时所说的端口号也是指的逻辑端口号

端口是一个软件结构，被客户程序或服务程序用来发送和接收数据，一台服务器有	256*256个端口。  端口号范围: 0 - 65535

0-1023是公认端口号，即已经公认定义或为将要公认定义的软件保留的

1024-65535是并没有公共定义的端口号，用户可以自己定义这些端口的作用。

端口与协议有关：TCP和UDP的端口互不相干



## 二、TCP编程

**什么是 TCP 协议**

TCP（Transmission Control Protocol 传输控制协议）是一种面向连接（连接导向）的、可靠的、基于IP的传输层协议。

弥补了IP协议的不足，属于一种较高级的协议，它实现了数据包的有力捆绑，通过排序和重传来确保数据传输的可靠(即数据的准确传输以及完整性)。

排序可以保证数据的读取是按照正确的格式进行，重传则保证了数据能够准确传送到目的地！ 

使用 TCP 协议通信是, 首先创建 TCP 连接, 主动发起连接的叫**客户端**, 被动响应连接的叫**服务器**

**比如:**
当我们在浏览器中访问新浪主页时，我们自己的计算机就是客户端，浏览器会主动向新浪的服务器发起连接。如果一切顺利，新浪的服务器接受了我们的连接，一个TCP连接就建立起来的，后面的通信就是发送网页内容了。

<img src="http://o7cqr8cfk.bkt.clouddn.com/markdown/1501302206747.png-yztcText">



###  2.1什么是`Socket`

`Socket`又称"套接字"，应用程序通常通过"套接字"向网络发出请求或者应答网络请求，使主机间或者一台计算机上的进程间可以通讯。

可以把`Socket`理解成类似插座的东西, 通过`Socket`就可以发送和接受数据了, 就像插座插上电器之后就可以向外提供电能了.

`TCP`编程的客户端和服务器端都是通过`Socket`来完成的.

***其实`UDP`协议通信也是使用的套接字, 和`TCP`协议稍有差别. `TCP`是面向连接的套接字, 而`UDP`是面向无连接的套接字.***

------

套接字的起源可以追溯到20世纪70年底, 他是加利福尼亚大学的伯克利版本 Unix(也成 BSD Unix) 的一部分. 因此, 有时你可能会听过将套接字称为伯克利套接字或 BSD 套接字.

套接字最初是为同一主机上的应用程序锁创建, 使得主机上一个程序(也叫一个进程)与另一个允许的程序进行通信. 这就是所谓的进程间通信(Inter Process Communication   IPC)

------

当我们知道如何通过网络端口访问到一个服务时，那么我们能够做什么呢？其实作为客户端来说，我们可以通过网络端口发送一个请求，然后得到服务器端反馈的信息。作为服务端，我们需要把我们的服务绑定到端口，并且在相应的端口上监听，当有客户端来访问时能够读取信息并且写入反馈信息。

在Go语言的net包中有一个类型TCPConn，这个类型可以用来作为客户端和服务器端交互的通道，他有两个主要的函数：

```go
func (c *TCPConn) Write(b []byte) (n int, err os.Error)
func (c *TCPConn) Read(b []byte) (n int, err os.Error)
```

TCPConn可以用在客户端和服务器端来读写数据。

还有我们需要知道一个TCPAddr类型，他表示一个TCP的地址信息，他的定义如下：

```go
type TCPAddr struct {
    IP IP
    Port int
}
```

在Go语言中通过ResolveTCPAddr获取一个TCPAddr

```go
func ResolveTCPAddr(net, addr string) (*TCPAddr, os.Error)
```

- net参数是"tcp4"、"tcp6"、"tcp"中的任意一个，分别表示TCPv4、TCPv6或者任意
- addr表示域名或者IP地址，例如"www.google.com:80" 或者"127.0.0.1:22".

TCP client
Go语言中通过net包中的DialTCP函数建议一个TCP连接，返回一个TCPConn类型，客户端和服务器段通过这个类型来进行数据交换。一般而言，客户端通过TCPConn写入请求信息发送到服务器端，读取服务器端反馈的信息。这个链接只有当任意一遍关闭了连接之后才失效，不然我们都可以一直使用。函数的定义如下：

```go
func DialTCP(net string, laddr, raddr *TCPAddr) (c *TCPConn, err os.Error)
```

- net参数是"tcp4"、"tcp6"、"tcp"中的任意一个，分别表示TCPv4、TCPv6或者任意
- laddr表示本机地址，一般设置为nil
- raddr表示远程的服务地址

### 2.2 TCP 服务端编程

创建服务器端程序，我们知道服务器端我们需要绑定服务到对应的端口，然后监听端口，当有客户端请求到达的时候接收客户端连接。net包中有相应的函数，函数定义如下：

```go
func ListenTCP(net string, laddr *TCPAddr) (l *TCPListener, err os.Error)
func (l *TCPListener) Accept() (c Conn, err os.Error)
```

示例代码：

```go
// TCPServer project main.go
package main

import (
    "fmt"
    "net"
    "os"
)

func main() {
    service := ":5000"
    tcpAddr, err := net.ResolveTCPAddr("tcp4", service) //在Go语言中通过ResolveTCPAddr获取一个TCPAddr
    checkErr(err)
    listener, err := net.ListenTCP("tcp", tcpAddr) // 绑定服务器端口，进行监听
    checkErr(err)
    for {
        conn, err := listener.Accept()
        if err != nil {
            continue
        }
        go handleClient(conn)
    }
}

func checkErr(err error) {
    if err != nil {
        fmt.Fprintf(os.Stderr, "Fatal error: %s", err.Error())
        os.Exit(1)
    }
}

func handleClient(conn net.Conn) {
    defer conn.Close()
    var buf [512]byte
    for {
        n, err := conn.Read(buf[0:])
        if err != nil {
            return
        }
        rAddr := conn.RemoteAddr()
        fmt.Println("Receive from client", rAddr.String(), string(buf[0:n]))
        _, err2 := conn.Write([]byte("Welcome client!"))
        if err2 != nil {
            return
        }
    }
}

```



### 2.3 TCP 客户端编程

客户端实例代码：

```go
// TCPClient project main.go
package main

import (
    "fmt"
    "net"
    "os"
)

func main() {
    var buf [512]byte
    if len(os.Args) != 2 {
        fmt.Fprintf(os.Stderr, "Usage: %s host:port ", os.Args[0])
        os.Exit(1)
    }
    service := os.Args[1]
    tcpAddr, err := net.ResolveTCPAddr("tcp4", service)
    checkErr(err)
    conn, err := net.DialTCP("tcp", nil, tcpAddr)
    defer conn.Close()
    checkErr(err)
    rAddr := conn.RemoteAddr()
    n, err := conn.Write([]byte("Hello server!"))
    checkErr(err)
    n, err = conn.Read(buf[0:])
    checkErr(err)
    fmt.Println("Reply from server ", rAddr.String(), string(buf[0:n]))
    os.Exit(0)
}

func checkErr(err error) {
    if err != nil {
        fmt.Fprintf(os.Stderr, "Fatal error: %s", err.Error())
        os.Exit(1)
    }
}

```



## 三、UDP编程

**`UDP`简介** 

`UDP`也叫用户数据报协议

`UDP`编程相比`TCP`编程简单了很多.

因为`UDP`不是面向连接的, 而是面向无连接的.

`TCP`是面向连接的, 客户端和服务端必须连接之后才能通讯, 就像打电话, 必须先接通才能通话.

`UDP`是面向无连接的, 一方负责发送数据\(客户端\), 只要知道对方\(接受数据:服务器\) 的地址就可以直接发数据了, 但是能不能达到就没有办法保证了.

**虽然用UDP传输面向无连接, 数据不可靠，但它的优点是和TCP比，速度快，对于不要求可靠到达的数据，就可以使用UDP协议。 比如局域网的视频同步, 使用 udp 是比较合适的:快, 延迟越小越好**

Go语言包中处理UDP Socket和TCP Socket不同的地方就是在服务器端处理多个客户端请求数据包的方式不同,UDP缺少了对客户端连接请求的Accept函数。其他基本几乎一模一样，只有TCP换成了UDP而已。UDP的几个主要函数如下所示：

```go
	func ResolveUDPAddr(net, addr string) (*UDPAddr, os.Error)
    func DialUDP(net string, laddr, raddr *UDPAddr) (c *UDPConn, err os.Error)
    func ListenUDP(net string, laddr *UDPAddr) (c *UDPConn, err os.Error)
    func (c *UDPConn) ReadFromUDP(b []byte) (n int, addr *UDPAddr, err os.Error)
    func (c *UDPConn) WriteToUDP(b []byte, addr *UDPAddr) (n int, err os.Error)
```



一个UDP的客户端代码如下所示,我们可以看到不同的就是TCP换成了UDP而已：

```go
package main

    import (
        "fmt"
        "net"
        "os"
    )

    func main() {
        if len(os.Args) != 2 {
            fmt.Fprintf(os.Stderr, "Usage: %s host:port", os.Args[0])
            os.Exit(1)
        }
        service := os.Args[1]
        udpAddr, err := net.ResolveUDPAddr("udp4", service)
        checkError(err)
        conn, err := net.DialUDP("udp", nil, udpAddr)
        checkError(err)
        _, err = conn.Write([]byte("anything"))
        checkError(err)
        var buf [512]byte
        n, err := conn.Read(buf[0:])
        checkError(err)
        fmt.Println(string(buf[0:n]))
        os.Exit(0)
    }
    func checkError(err error) {
        if err != nil {
            fmt.Fprintf(os.Stderr, "Fatal error ", err.Error())
            os.Exit(1)
        }
    }
```



### 3.2 UDP服务器编程

我们来看一下UDP服务器端如何来处理：

```go
package main

    import (
        "fmt"
        "net"
        "os"
        "time"
    )

    func main() {
        service := ":1200"
        udpAddr, err := net.ResolveUDPAddr("udp4", service)
        checkError(err)
        conn, err := net.ListenUDP("udp", udpAddr)
        checkError(err)
        for {
            handleClient(conn)
        }
    }
    func handleClient(conn *net.UDPConn) {
        var buf [512]byte
        _, addr, err := conn.ReadFromUDP(buf[0:])
        if err != nil {
            return
        }
        daytime := time.Now().String()
        conn.WriteToUDP([]byte(daytime), addr)
    }
    func checkError(err error) {
        if err != nil {
            fmt.Fprintf(os.Stderr, "Fatal error ", err.Error())
            os.Exit(1)
        }
    }
```



## 四、net模块其他属性和函数

控制TCP连接

TCP连接有很多控制函数，我们平常用到比较多的有如下几个函数：

```
func (c *TCPConn) SetTimeout(nsec int64) os.Error
func (c *TCPConn) SetKeepAlive(keepalive bool) os.Error

```

第一个函数用来设置超时时间，客户端和服务器端都适用，当超过设置的时间时那么该链接就失效。

第二个函数用来设置客户端是否和服务器端一直保持着连接，即使没有任何的数据发送

更多的内容请查看`net`包的文档。

阻塞Dial：

```go
conn, err := net.Dial("tcp", "google.com:80")
if err != nil {
    //handle error
}
// read or write on conn
```

或是带上超时机制的Dial：

```go
conn, err := net.DialTimeout("tcp", ":8080", 2 * time.Second)
if err != nil {
    //handle error
}
// read or write on conn
```