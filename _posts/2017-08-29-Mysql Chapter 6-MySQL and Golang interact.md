---
layout: post
title:  "MySQL数据库第6章-MySQL和Golang交互"
categories: MySQL Golang
tags: MySQL,关系型数据库,Golang
---

* content
{:toc}
# MySQL和python交互

## 一、安装mysql模块

**go语言连接mysql简介**
​    go官方仅提供了database package，database package下有两个包sql，sql/driver。这两个包用来定义操作数据库的接口，这就保证了无论使用哪种数据库，他们的操作方式都是相同的。
​    但go官方并没有提供连接数据库的driver，如果要操作数据库，还需要第三方的driver 包，最常用的有：
​    <https://github.com/Go-SQL-Driver/MySQL>支持database/sql，全部采用go写。

1.下载安装

	A：Windows系统下载
	
	使用github.com/go-sql-driver/mysql这个驱动包，打开cmd窗口输入：go get 	github.com/go-sql-driver/mysql，会下载到你的GOPATH路径的src 下：
	
	![](http://om1c35wrq.bkt.clouddn.com/101mysql%E9%A9%B1%E5%8A%A81.bmp)

如果没有安装git，需要先安装git。否则安装失败。



下载后在有如下目录。

![](http://om1c35wrq.bkt.clouddn.com/105%E9%A9%B1%E5%8A%A8.bmp)



	B：Ubuntu下安装方式：

　　执行下面两个命令：

　　　　下载：go get github.com/Go-SQL-Driver/MySQL
 　　　　安装：go install github.com/Go-SQL-Driver/MySQL
　　安装完成以后的文件截图

![](http://om1c35wrq.bkt.clouddn.com/100mysql.png)



## 二、导入包

import (
​        　　"database/sql"
​        　　_"github.com/Go-SQL-Driver/MySQL"
　　)

![](http://om1c35wrq.bkt.clouddn.com/103daorubao.png)

>1. database/sql，是golang的标准库之一，它提供了一系列接口方法，用于访问关系数据库。它并不会提供数据库特有的方法，那些特有的方法交给数据库驱动去实现。
>2. 我们正在加载的驱动是匿名的，将其限定符别名为_,因此我们的代码中没有一个到处的名称可见。

下面例子中的表结构：

````
CREATE TABLE userinfo (
	uid int(10) NOT NULL AUTO_INCREMENT,
	username varchar(64) DEFAULT NULL,
	departname varchar(64) DEFAULT NULL,
	created date DEFAULT NULL,
	PRIMARY KEY (uid)
) ;
````

 

![](http://om1c35wrq.bkt.clouddn.com/119%E5%88%9B%E5%BB%BA%E8%A1%A8.png)

## 三、连接

Open函数：

```go
db, err := sql.Open("mysql", "用户名:密码@tcp(IP:端口)/数据库?charset=utf8")
```

例如：db, err := sql.Open("mysql", "root:111111@tcp(127.0.0.1:3306)/test?charset=utf8")



## 四、增删改

有两种方法：
1.直接使用Exec函数添加

```go
func (db *DB) Exec(query string, args ...interface{}) (Result, error)
```

示例代码：

```go
result, err := db.Exec("INSERT INTO userinfo (username, departname, created) VALUES (?, ?, ?)","lily","销售","2016-06-21")
```


2.首先使用Prepare获得stmt，然后调用Exec添加

```go
 func (db *DB) Prepare(query string) (*Stmt, error)
```

示例代码：

```go
stmt, err := db.Prepare("INSERT userinfo SET username=?,departname=?,created=?")

result, err := stmt.Exec("zhja", "研发", "2016-06-17")
```

获取影响数据库的行数，可以根据该数值判断是否插入或删除或修改成功。

```go
 count, err := result.RowsAffected()
```



获得刚刚添加数据的自增ID

```go
id, err := result.LastInsertId()
```



## 五、查询

**查询**
**查询单条数据，QueryRow 函数**

```go
func (db *DB) QueryRow(query string, args ...interface{}) *Row
```

示例代码：

```go
var username, departname, created string
err := db.QueryRow("SELECT username,departname,created FROM userinfo WHERE uid=?", 3).Scan(&username, &departname, &created)

```

代码截图：

![](http://om1c35wrq.bkt.clouddn.com/120query.png)

扫描并复制当前行中每一列的值，但是要求行必须与行中的列数相同。

```go
func (rs *Rows) Scan(dest ...interface{}) error
```



**查询多条数据，并遍历**
　　Query 获取数据，for xxx.Next() 遍历数据

```go
func (db *DB) Query(query string, args ...interface{}) (*Rows, error)
```

```go
  func (rs *Rows) Next() bool
```




![](http://om1c35wrq.bkt.clouddn.com/121query.png)


