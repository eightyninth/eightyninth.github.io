---
title: 数据库 1
date: 2022/8/25 下午10:18
tags: [数据库, 校招]
categories: [校招]
---

> SQL: Structured Query Language
>
> RDBMS: Relational Database Management System
> 

# 查看命令

1. 查看数据库

```bash
use your_database_name;
```

2. 设置使用的字符集

```bash
set names utf-8;
```

# select

用于从数据库中选取数据

```bash
# 1.
select * 
from table_name;

# 2.
select column_name, column_name 
from table_name;
```
# select distinct

用于返回唯一不同的值(去重子集)。

```bash
select distinct column_name, column_name 
from table_name;
```

# where
用于过滤记录

```bash
select column_name, column_name 
from table_name 
where column_name operator value;
```

|   运算符   |      描述       |
|:-------:|:-------------:|
|    =    |      等于       |
|  <>/!=  |      不等于      |
|    >    |      大于       |
|    <    |      小于       |
|   >=    |     大于等于      |
|   <=    |     小于等于      |
| between |    在某个范围内     |
|  like   |    搜索某种模式     |
|   in    | 指定针对某个列的多个可能值 |

# and / or
用于基于一个以上的条件对记录进行过滤

如果所有条件都成立，and运算符显示一条记录。

如果有一个条件成立，or运算符显示一条记录。

```bash
select * 
from table_name
where column_name operator value 
or / and column_name operator value;
```

# order by
用于对结果集进行排序

对结果集按照一个列或者多个列进行排序。

默认按照升序对结果进行排序。

需要按照降序排列， 使用desc关键字。

```bash
select column_name, column_name 
from table_name
order by column_name, column_name asc / desc;
```

# insert into
用于向表中插入新纪录。

```bash
# 1. 无需指定要插入数据的列名,只需要提供被插入的值即可;
insert into table_name
values (value1, value2, ..., valuen);

# 2. 需要指定列名及被插入的值.
insert into table_name (column1, column2, ..., columnn)
values (value1, value2, ..., valuen);
```
# update
用于更新表中已存在的记录。

```bash
update table_name
set column1=value1, column2=value2
where some_column = some_value;
```

# delete
用于删除表中的记录

```bash
delete from table_name
where some_column=some_value;
```

# select top / limit / rownum

select top 
> select top是SQL Server使用的语句，并非所有数据库系统都支持。mysql支持limit

# create database

# alter database

# create table

# alter table

# drop table

# create index

# drop index