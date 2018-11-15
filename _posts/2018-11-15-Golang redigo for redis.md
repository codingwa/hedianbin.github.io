---
layout: post
title:  "Golang操作redis指南"
categories: Golang redis
tags: Golang redis redigo
---

* content
{:toc}


# Golang操作redis指南

### 相关模块以及安装方式

[redigo模块](https://link.jianshu.com?t=https%3A%2F%2Fgithub.com%2Fgomodule%2Fredigo)   
 [redis-cluster客户端实现](https://link.jianshu.com?t=https%3A%2F%2Fgithub.com%2Fmna%2Fredisc%2F)
 [go-redis模块](https://link.jianshu.com?t=https%3A%2F%2Fgithub.com%2Fgo-redis%2Fredis%2F)

```
注意:如下操作使用redigo模块进行redis的操作
```

**安装使用**

```
$ go get -v github.com/gomodule/redigo
$ cat redis-conn.go
/**
 * @File Name: redis-conn.go
 * @Author: xxbandy @http://xxbandy.github.io
 * @Email:
 * @Create Date: 2018-04-04 07:04:57
 * @Last Modified: 2018-04-08 14:04:38
 * @Description:
 */
package main
import (
    "fmt"
    "os"
    redis "github.com/gomodule/redigo/redis"
)
//自定义的redis实例的端口为32771
func main() {
    //使用redis封装的Dial进行tcp连接
    c,err := redis.Dial("tcp","localhost:32771")
    errCheck(err)

    defer c.Close()

    //对本次连接进行set操作
    _,setErr := c.Do("set","url","xxbandy.github.io")
    errCheck(setErr)

    //使用redis的string类型获取set的k/v信息
    r,getErr := redis.String(c.Do("get","url"))
    errCheck(getErr)
    fmt.Println(r)

}

func errCheck(err error) {
    if err != nil {
        fmt.Println("sorry,has some error:",err)
        os.Exit(-1)
        }
    }
    
# 执行redis的set和get的操作
$ go run redis-conn.go
xxbandy.github.io
```



### redigo模块的详细使用

#### 连接

`Conn`接口是redis操作过程中比较重要的接口。应用一般通过调用`Dial、DialWithTimeout、NewConn`函数来创建一个链接。
 `注意：`应用必须在操作redis结束之后去调用该连接的`Close`方法来关闭连接，以防止资源消耗以及其他问题。

**Conn接口下的相关方法**

```
type Conn interface {
    // 关闭链接
    Close() error

    // 当链接不可用时返回非空值
    Err() error

    //Do方法向redis服务端发送命令并返回接收到响应
    Do(commandName string, args ...interface{}) (reply interface{}, err error)

    //Send 将相关的命令写入客户端的buffer中
    Send(commandName string, args ...interface{}) error

    // Flush 将客户端的输出缓冲内容刷新到redis服务器端
    Flush() error

    // Receive 从redis服务端接收单个回复
    Receive() (reply interface{}, err error)
}
```

**常用的创建Conn的方式**

```
# 使用Dial函数
# func Dial(network, address string, options ...DialOption) (Conn, error)
/*可选的参数
func DialConnectTimeout(d time.Duration) DialOption
func DialDatabase(db int) DialOption
func DialKeepAlive(d time.Duration) DialOption
func DialNetDial(dial func(network, addr string) (net.Conn, error)) DialOption
func DialPassword(password string) DialOption
func DialReadTimeout(d time.Duration) DialOption
func DialWriteTimeout(d time.Duration) DialOption
*/
c,err := redis.Dial("tcp","localhost:32771")
defer c.Close()



# 使用DialTimeout函数(默认增加了连接、读写超时时间)
// func DialTimeout(network, address string, connectTimeout, readTimeout, writeTimeout time.Duration) (Conn, error)


# 使用NewConn函数(将一个网络连接转换成redis连接)
// func NewConn(netConn net.Conn, readTimeout, writeTimeout time.Duration) Conn
```

#### redis命令的执行

在上面`Conn`的接口中可以看到有一个`Do`方法，可以用来执行redis命令操作。

**Do方法定义**

```
//使用Conn的Do方法来操作redis命令
Do(commandName string, args ...interface{}) (reply interface{}, err error)

//示例
n, err := conn.Do("APPEND", "key", "value")
```

#### redis连接和命令执行的简单示例

简单示例：

```
$ cat redis-conn.go
package main
import (
    "fmt"
    "os"
    "time"
    redis "github.com/gomodule/redigo/redis"
)
func main() {
    //使用redis封装的Dial进行tcp连接.设置长连接时间为1s,连接超时时间为5s,读写超时均为1s，并设置密码进行连接
    c,err := redis.Dial("tcp","localhost:32771",redis.DialKeepAlive(1*time.Second),redis.DialPassword("123qweasd"),redis.DialConnectTimeout(5*time.Second),redis.DialReadTimeout(1*time.Second),redis.DialWriteTimeout(1*time.Second))
    errCheck(err)

    defer c.Close()
    
    //使用redis的string类型获取set的k/v信息
    r,getErr := redis.String(c.Do("get","url"))
    errCheck(getErr)
    fmt.Println(r)

}

func errCheck(err error) {
    if err != nil {
        fmt.Println("sorry,has some error:",err)
        os.Exit(-1)
        }
    }
    
$ go run redis-conn.go
xxbandy.github.io

# 由于redis服务器设置了密码，如果密码错误会报如下异常
$ go run redis-conn.go
sorry,has some error: ERR invalid password
exit status 255
```

#### 使用Pipelining操作redis

`注意:`建议使用redis自身包含的命令进行批量操作而不是使用pipelining，比如`mset、mget、hmset、hmget`等等。原子性可能会更好一些

`Conn`会使用`Send、Flush、Receive`来支持pipeline操作。

```
Send(commandName string, args ...interface{}) error
Flush() error
Receive() (reply interface{}, err error)
```

`Send`会写入命令到连接的输出缓冲区里。`Flush`会将输出缓冲区中的数据刷新到服务端。`Receive`回去荣服务端读取单个响应。

```
c.Send("SET", "foo", "bar")
c.Send("GET", "foo")
c.Flush()
c.Receive()
v, err = c.Receive()
```

其实，在`Do`方法中会包含`Send/Flush/Receive`等方法。

使用`Send`和`Do`方法来实现pipeline。

```
c.Send("MULTI")
c.Send("INCR", "foo")
c.Send("INCR", "bar")
r, err := c.Do("EXEC")
fmt.Println(r) //[1,1]
```

#### 并发

一般并发访问redis，建议使用安全线程池去获取连接，从一个goroutine中使用和释放一个链接。正如前面提到的，从线程池中获取的连接会有并发限制。

#### 发布/订阅模式(Publish and Subscribe)

使用`Send/Flush/Receive`方法去实现Pub/Sub订阅。

```
c.Send("SUBSCRIBE","ANSIBLE-KEY")
c.Flush()
for {
    reply,err := c.Receive()
    if err != nil {
        return err
    }
    //process pushed message
}
```

`PubSubConn`类型使用`convenience`方法包装了一个`Conn`来实现了订阅。`Subscribe/PSubscribe/Unsubscribe/PUnsubscribe`方法会send并且flush一个订阅管理命令。接收方法会将push的消息转换成一个可以进行`switch`的合适类型。

```
psc := redis.PubSubConn{Conn: c}

psc.Subscribe("example")
for {
    switch v := psc.Receive().(type) {
    case redis.Message:
        fmt.Printf("%s: message: %s\n", v.Channel, v.Data)
    case redis.Subscription:
        fmt.Printf("%s: %s %d\n", v.Channel, v.Kind, v.Count)
    case error:
        return v
    }
}
```

### Redigo操作redis的几种场景示例

#### 使用`Args`接口数组操作处理批量操作

相关的函数和方法：

```
//func Values(reply interface{}, err error) ([]interface{}, error)
//type Args []interface{}

# 类似于hgetall和config get的输出格式
//func StringMap(result interface{}, err error) (map[string]string, error)

# 类似于hmget 类型
//func Strings(reply interface{}, err error) ([]string, error)

# 从src中扫描k/v到一个结构体中。一般使用较多的就是hgetall和config get命令
//func ScanStruct(src []interface{}, dest interface{}) error
```

`hmset`和`hgetall`命令的使用：

```
$  cat redis-hash.go
/**
 * @File Name: redis-hash.go
 * @Author: xxbandy @http://xxbandy.github.io
 * @Email:
 * @Create Date: 2018-04-04 08:04:55
 * @Last Modified: 2018-04-08 22:04:47
 * @Description:
 */
package main
import (
    "fmt"
    "time"
    "os"
    "github.com/gomodule/redigo/redis"
)
//构造一个链接函数，如果没有密码，passwd为空字符串
func redisConn(ip,port,passwd string) (redis.Conn, error) {
    c,err := redis.Dial("tcp",
        ip+":"+port,
        redis.DialConnectTimeout(5*time.Second),
        redis.DialReadTimeout(1*time.Second),
        redis.DialWriteTimeout(1*time.Second),
        redis.DialPassword(passwd),
        redis.DialKeepAlive(1*time.Second),
        )
    return c,err
}

//构造一个错误检查函数
func errCheck(tp string,err error) {
    if err != nil {
        fmt.Printf("sorry,has some error for %s.\r\n",tp,err)
        os.Exit(-1)
    }
}

//构造实际场景的hash结构体
var p1,p2 struct {
    Description  string `redis:"description"`
    Url          string `redis:"url"`
    Author       string `redis:"author"`
}



//主函数
func main() {
    c,cErr := redisConn("localhost","32771","123qweasd")
    errCheck("Conn",cErr)

    defer c.Close()
    p1.Description = "my blog"
    p1.Url = "http://xxbandy.github.io"
    p1.Author = "bgbiao"

    _,hmsetErr := c.Do("hmset",redis.Args{}.Add("hao123").AddFlat(&p1)...)
    errCheck("hmset",hmsetErr)

    m := map[string]string{
        "description":    "oschina",
        "url":            "http://my.oschina.net/myblog",
        "author":         "xxbandy",
    }

    _,hmset1Err := c.Do("hmset",redis.Args{}.Add("hao").AddFlat(m)...)
    errCheck("hmset1",hmset1Err)

    for _,key := range []string{"hao123","hao"} {
        v, err := redis.Values(c.Do("hgetall",key))
        errCheck("hmgetV",err)
        //等同于hgetall的输出类型，输出字符串为k/v类型
        //hashV,_ := redis.StringMap(c.Do("hgetall",key))
        //fmt.Println(hashV)
        //等同于hmget 的输出类型，输出字符串到一个字符串列表
        hashV2,_ := redis.Strings(c.Do("hmget",key,"description","url","author"))
        for _,hashv := range hashV2 {
                fmt.Println(hashv)
            }
        if err := redis.ScanStruct(v,&p2);err != nil {
            fmt.Println(err)
            return
        }
    fmt.Printf("%+v\n",p2)


    }

}
$ tcp-http go run redis-hash.go
{Description:my blog Url:http://xxbandy.github.io Author:bgbiao}
{Description:oschina Url:http://my.oschina.net/myblog Author:xxbandy}

bash-4.1# redis-cli -a 123qweasd hgetall hao
1) "description"
2) "oschina"
3) "url"
4) "http://my.oschina.net/myblog"
5) "author"
6) "xxbandy"
bash-4.1# redis-cli -a 123qweasd hgetall hao123
1) "description"
2) "my blog"
3) "url"
4) "http://xxbandy.github.io"
5) "author"
6) "bgbiao"
```

`hset`和`hget的`使用：

```
// core functions
_,err := c.Do("hset","books","name","golang")
if r,err := redis.String("hget","books","name");err == nil {
    fmt.Println("book name:",r)
}
```

#### 使用`String和Int`获取字符串类型的输出

也就是`set、get、mset、mget`之类的命令。
 [string相关命令](https://link.jianshu.com?t=http%3A%2F%2Fredisdoc.com%2Fstring%2Findex.html)

```
注意:当获取数字类型的key时需要使用redis.Int()获取数字类型的输出结果
$ cat redis-string.go
....
func main() {
    c,connErr := redisConn("localhost","32771","123qweasd")
    errCheck("connErr",connErr)

    getV,_ := redis.String(c.Do("get","name"))
    getV2,_ := redis.Int(c.Do("get","id"))
    fmt.Println(getV,getV2)
}
$ go run redis-string.go
xxb 1214

127.0.0.1:6379> mget name id
1) "xxb"
2) "1214"
```

`mset、mget、expire`之类的使用：

```
// core functions
_,err = c.Do("mset","id",100,"fn",200)

_,expireErr = c.Do("expire","id",10)

if r,err := redis.Ints(c.Do("mget","id","fn")); err == nil {
    for _,v := range {
        fmt.Println("value:",v)
    }
}
```

`lpush、lpop、rpush、rpop`等相关命令操作:

```
// core functions
_,err = c.Do("lpush","updateid","0407","0408","0409")

r,err ：= redis.String(c.Do("lpop","updateid"))
```

#### 使用`redis`的安全链连接池

使用连接池可以高效的管理redis的连接，并可以方便地控制redis的并发性能。

相关的结构体和方法：

```
type Pool struct {
    //创建一个tcp链接的匿名函数
    Dial func() (Conn, error)
    //可选的函数，用来对之前使用过的空闲链接进行安全检查
    TestOnBorrow func(c Conn, t time.Time) error
    //最大可用链接数
    MaxIdle int
    //在给定的时间，最大可分配的连接数。为0则不限制
    MaxActive int
    //空闲链接的关闭时间，如果为空，空闲链接不会被关闭
    IdleTimeout time.Duration
    //如果Wait为true并且进行MaxActive限制了，Get()将会等待链接被返回
    Wait bool
    //关闭老链接的时间区间。如果为空则不会在生命周期内关闭链接
    MaxConnLifetime time.Duration
}

//创建一个Pool结构体
func NewPool(newFn func() (Conn, error), maxIdle int) *Pool

// 获取pool的连接数，包含空闲链接和使用连接 
func (p *Pool) ActiveCount() int


//关闭并释放pool中的资源
func (p *Pool) Close() error

//从pool中获取一个连接
func (p *Pool) Get() Conn

// 获取空闲链接数
func (p *Pool) IdleCount() int

// 获取连接池的状态信息
func (p *Pool) Stats() PoolStats
type PoolStats struct {
    ActiveCount int
    IdleCount   int
}
```

示例:

```
$ cat redis-pool.go
package main
import (
    "fmt"
    "time"
    "os"
    "github.com/gomodule/redigo/redis"
)
//构造一个链接函数，如果没有密码，passwd为空字符串
func redisConn(ip,port,passwd string) (redis.Conn, error) {
    c,err := redis.Dial("tcp",
        ip+":"+port,
        redis.DialConnectTimeout(5*time.Second),
        redis.DialReadTimeout(1*time.Second),
        redis.DialWriteTimeout(1*time.Second),
        redis.DialPassword(passwd),
        redis.DialKeepAlive(1*time.Second),
        )
    return c,err
}

//构造一个错误检查函数
func errCheck(tp string,err error) {
    if err != nil {
        fmt.Printf("sorry,has some error for %s.\r\n",tp,err)
        os.Exit(-1)
    }
}

//构造一个连接池
//url为包装了redis的连接参数ip,port,passwd
func newPool(ip,port,passwd string) *redis.Pool {
    return &redis.Pool{
        MaxIdle:            5,    //定义redis连接池中最大的空闲链接为3
        MaxActive:          18,    //在给定时间已分配的最大连接数(限制并发数)
        IdleTimeout:        240 * time.Second,
        MaxConnLifetime:    300 * time.Second,
        Dial:               func() (redis.Conn,error) { return redisConn(ip,port,passwd) },
    }
}


func main() {
    //使用newPool构建一个redis连接池
    pool := newPool("localhost","32771","123qweasd")
    defer pool.Close()
    for i := 0;i <= 4;i++ {
      go func() {
        //从pool里面获取一个可用的redis连接
        c := pool.Get()
        defer c.Close()
        //mset mget
        fmt.Printf("ActiveCount:%d IdleCount:%d\r\n",pool.Stats().ActiveCount,pool.Stats().IdleCount)
        _,setErr := c.Do("mset","name","biaoge","url","http://xxbandy.github.io")
        errCheck("setErr",setErr)
        if r,mgetErr := redis.Strings(c.Do("mget","name","url")); mgetErr == nil {
            for _,v := range r {
                fmt.Println("mget ",v)
            }
        }
      }()
    }

    time.Sleep(1*time.Second)
}

# 使用连接池进行获取连接并处理redis操作
$ go run redis-pool.go
ActiveCount:5 IdleCount:0
ActiveCount:5 IdleCount:0
ActiveCount:5 IdleCount:0
ActiveCount:5 IdleCount:0
ActiveCount:5 IdleCount:0
mget  biaoge
mget  http://xxbandy.github.io
mget  biaoge
mget  http://xxbandy.github.io
mget  biaoge
mget  http://xxbandy.github.io
mget  biaoge
mget  http://xxbandy.github.io
mget  biaoge
mget  http://xxbandy.github.io
```

#### 使用`redis`的发布订阅模式

相关的结构体以及方法：

```
//PubSubConn结构体
type PubSubConn struct {
    Conn    Conn
}

//使用redis链接初始化一个pubsub链接
psc := redis.PubSubConn{Conn:redis.conn}

//pubsub链接的相关方法
//关闭pubsub链接
func (c PubSubConn) Close() error
//批量订阅(按模式订阅)
func (c PubSubConn) PSubscribe(channel ...interface{}) error

//取消订阅
func (c PubSubConn) PUnsubscribe(channel ...interface{}) error

// Receive返回消费的信息作为Subscription, Message, Pong or error.返回值建议直接在switch中使用
func (c PubSubConn) Receive() interface{}
type Subscription struct {
    Kind    string  //订阅,取消订阅
    Channel string  //被改变的channel
    //订阅的链接数量
    Count   int
}
type Message struct {
    // 初始的channel
    Channel string
    // 匹配的模式
    Pattern string
    // channel的消息体
    Data []byte
}
type



//订阅和取消订阅
func (c PubSubConn) Subscribe(channel ...interface{}) error
func (c PubSubConn) Unsubscribe(channel ...interface{}) error
```

测试示例：

```
package main

import (
    "context"
    "fmt"
    "time"

    "github.com/gomodule/redigo/redis"
)

//
func listenPubSubChannels(ctx context.Context, redisServerAddr string,
    onStart func() error,
    onMessage func(channel string, data []byte) error,
    channels ...string) error {

    //一分钟的心跳检测
    const healthCheckPeriod = time.Minute

    //构建一个redis链接
    c, err := redis.Dial("tcp", redisServerAddr,
        redis.DialReadTimeout(healthCheckPeriod+10*time.Second),
        redis.DialWriteTimeout(10*time.Second))
    if err != nil {
        return err
    }
    defer c.Close()

    //构建一个pubsub链接
    psc := redis.PubSubConn{Conn: c}

    //订阅channels
    if err := psc.Subscribe(redis.Args{}.AddFlat(channels)...); err != nil {
        return err
    }
    //构造chan来检测通知状态
    done := make(chan error, 1)

    //启动一个goroutine来接受来自server端的通知
    go func() {
        for {
            //使用interface{}.(type) 来获取对应的类型，并借助switch和case进行interface{}的类型判断
            switch n := psc.Receive().(type) {
            case error:
                done <- n
                return
            case redis.Message:
                if err := onMessage(n.Channel, n.Data); err != nil {
                    done <- err
                    return
                }
            case redis.Subscription:
                switch n.Count {
                case len(channels):
                    // Notify application when all channels are subscribed.
                    if err := onStart(); err != nil {
                        done <- err
                        return
                    }
                case 0:
                    // Return from the goroutine when all channels are unsubscribed.
                    done <- nil
                    return
                }
            }
        }
    }()

    ticker := time.NewTicker(healthCheckPeriod)
    defer ticker.Stop()
loop:
    for err == nil {
        select {
        case <-ticker.C:
            // Send ping to test health of connection and server. If
            // corresponding pong is not received, then receive on the
            // connection will timeout and the receive goroutine will exit.
            if err = psc.Ping(""); err != nil {
                break loop
            }
        case <-ctx.Done():
            break loop
        case err := <-done:
            // Return error from the receive goroutine.
            return err
        }
    }

    // Signal the receiving goroutine to exit by unsubscribing from all channels.
    psc.Unsubscribe()

    // Wait for goroutine to complete.
    return <-done
}

func publish(redisServerAddr string) {
    c, err := redis.Dial("tcp",redisServerAddr)
    if err != nil {
        fmt.Println(err)
        return
    }
    defer c.Close()

    c.Do("PUBLISH", "c1", "hello")
}

// This example shows how receive pubsub notifications with cancelation and
// health checks.
func main() {
    redisServerAddr := "localhost:32771"

    ctx, cancel := context.WithCancel(context.Background())

    //ctx和start callback很好的解决了丢失的消息的填充(使用goroutine来占住message类型的消息)
    listenErr := listenPubSubChannels(ctx,
        redisServerAddr,
        func() error {
            // The start callback is a good place to backfill missed
            // notifications. For the purpose of this example, a goroutine is
            // started to send notifications.
            go publish(redisServerAddr)
            return nil
        },
        func(channel string, message []byte) error {
            fmt.Printf("channel: %s, message: %s\n", channel, message)

            // For the purpose of this example, cancel the listener's context
            // after receiving last message sent by publish().
            if string(message) == "goodbye" {
                cancel()
            }
            return nil
        },
        "ansible-key")

    if listenErr != nil {
        fmt.Println(listenErr)
        return
    }

}
```

