---
layout: post
title:  "MySQL数据库第5章-MySQL和Python交互"
categories: MySQL Python
tags: MySQL,关系型数据库,Python
---

* content
{:toc}
# MySQL和python交互

## 一、安装mysql模块

打开命令行，输入以下内容

```python
pip install pymysql3
```

> 在python2中，python操作MySQL数据库使用的是MySQLdb模块，但是在python3中我们要使用pymysql模块。
>
> 所以，如果是python2.7版本：pip install python-mysqldb，然后import MySQLdb

然后在python文件中引入模块

```python
import pymysql
```

测试代码：

```python
import pymysql
conn = pymysql.connect(host='127.0.0.1', port=3306, user='root', passwd='meditation',db='mysql')  # passwd换成自己的mysql密码
cursor = conn.cursor()  
cursor.execute("SELECT VERSION()")  
row = cursor.fetchone()  
print("MySQL server version:", row[0])
cursor.close()  
conn.close()  
```



## 二、Connection对象

用于建立与数据库的链接

创建对象：调用connect()方法

```python
conn = connect(参数列表)
```

参数 说明：

- 参数host：连接的mysql主机，如果本机是'localhost'
- 参数port：连接的mysql主机的端口，默认是3306
- 参数db：数据库的名称
- 参数user：连接的用户名
- 参数password：连接的密码
- 参数charset：通信采用的编码方式，默认是'gb2312'，要求与数据库创建时指定的编码一致，否则中文会乱码

#### 对象的方法

- close()关闭连接
- commit()事务，所以需要提交才会生效
- rollback()事务，放弃之前的操作
- cursor()返回Cursor对象，用于执行sql语句并获得结果

## 三、Cursor对象

- 执行sql语句
- 创建对象：调用Connection对象的cursor()方法

```python
cursor1=conn.cursor()
```

#### 对象的方法

- close()关闭
- execute(operation [, parameters ])执行语句，返回受影响的行数
- fetchone()执行查询语句时，获取查询结果集的第一个行数据，返回一个元组
- fetchall()执行查询时，获取结果集的所有行，一行构成一个元组，再将这些元组装入一个元组返回
- scroll(value[,mode])将行指针移动到某个位置
  - mode表示移动的方式
  - mode的默认值为relative，表示基于当前行移动到value，value为正则向下移动，value为负则向上移动
  - mode的值为absolute，表示基于第一条数据的位置，第一条数据的位置为0

#### 对象的属性

- rowcount只读属性，表示最近一次execute()执行后受影响的行数
- connection获得当前连接对象

## 四、增删改

### 4.1 增加一条数据

- 创建testInsert.py文件，向学生表中插入一条数据

```python
# 1.导入模块
import pymysql
try:
    # 2.创建连接对象
    conn = pymysql.connect(host="127.0.0.1", port=3306, user="root", passwd="hanru1314", db="ruby",charset="utf8")
    # 3.获取cursor对象
    cs1 = conn.cursor()
    # 4.准备sql语句
    sql = r"insert into students(id, name,age,sex,birthday) values(0,'王二狗',20,'男','1900-09-09')"
    # 5.执行插入数据
    count = cs1.execute(sql)
    print(count)
    # 6.执行提交
    conn.commit()
    
except Exception as result:
    print(str(result))
finally:
  	# 7.关闭 资源
    cs1.close()
    conn.close()
```

### 4.2 修改一条数据

- 创建testUpdate.py文件，修改学生表的一条数据

```python
# 1.导入模块
import pymysql
try:
    # 2.创建连接对象
    conn = pymysql.connect(host="127.0.0.1", port=3306, user="root", passwd="hanru1314", db="ruby",charset="utf8")
    # 3.获取cursor对象
    cs1 = conn.cursor()
    # 4.准备sql语句
    sql = r"update students set name='李小花', sex='女',birthday='2000-10-10' where id=1"
    # 5.执行修改数据
    count = cs1.execute(sql)
    print(count)
    # 6.执行提交
    conn.commit()
    
except Exception as result:
    print(str(result))
finally:
  	# 7.关闭 资源
    cs1.close()
    conn.close()
```

### 4.3 删除一条数据

- 创建testDelete.py文件，删除学生表的一条数据

```python
# 1.导入模块
import pymysql
try:
    # 2.创建连接对象
    conn = pymysql.connect(host="127.0.0.1", port=3306, user="root", passwd="hanru1314", db="ruby",charset="utf8")
    # 3.获取cursor对象
    cs1 = conn.cursor()
    # 4.准备sql语句
    sql = r"delete from students WHERE id=3"
    # 5.执行删除数据
    count = cs1.execute(sql)
    print(count)
    # 6.执行提交
    conn.commit()
    
except Exception as result:
    print(str(result))
finally:
  	# 7.关闭 资源
    cs1.close()
    conn.close()
```

### 4.4 其它语句

- cursor对象的execute()方法，也可以用于执行create table等语句
- 建议在开发之初，就创建好数据库表结构，不要在这里执行

## 五、查询

### 5.1 查询一条数据

```python
# 1.导入模块
import pymysql
try:
    # 2.创建连接对象
    conn = pymysql.connect(host="127.0.0.1", port=3306, user="root", passwd="hanru1314", db="ruby",charset="utf8")
    # 3.获取cursor对象
    cs1 = conn.cursor()
    # 4.准备sql语句
    sql = "select id, name, age, sex, birthday from students where id=1"
    # 5.执行查询数据
    cs1.execute(sql)
    result = cs1.fetchone()
    print(result)

except Exception as result:
    print(str(result))
finally:
    # 7.关闭 资源
    cs1.close()
    conn.close()

```



### 5.2 查询多条数据

```python
# 1.导入模块
import pymysql
try:
    # 2.创建连接对象
    conn = pymysql.connect(host="127.0.0.1", port=3306, user="root", passwd="hanru1314", db="ruby",charset="utf8")
    # 3.获取cursor对象
    cs1 = conn.cursor()
    # 4.准备sql语句
    sql = "select id, name, age, sex, birthday from students"
    # 5.执行查询数据
    cs1.execute(sql)
    result = cs1.fetchall()
    print(result)

except Exception as result:
    print(str(result))
finally:
    # 7.关闭 资源
    cs1.close()
    conn.close()
```



## 六、sql语句参数化

- 创建testInsertParam.py文件，向学生表中插入一条数据

```python
import pymysql

conn=pymysql.connect(host='localhost',port=3306,db='ruby',user='root',passwd='hanru1314',charset='utf8')
cs1=conn.cursor()
sname=input("请输入学生姓名：")
sage = input("请输入学生年龄：")
sex = input("请输入学生性别：")
sbirtyday=input("请输入学生生日：")
sage = int(sage)
print(type(sage))
params=(sname, sage, sex, sbirtyday)  # 列表或元组
sql="insert into students(name,age,sex,birthday) values(%s,%s,%s,%s)"
# java insert into students(....) values(?,?,?,?,?)
count=cs1.execute(sql, params)
print(count)
conn.commit()
cs1.close()
conn.close()
```

## 七、封装

- 观察前面的文件发现，除了sql语句及参数不同，其它语句都是一样的
- 创建MysqlHelper.py文件，定义类
- ​