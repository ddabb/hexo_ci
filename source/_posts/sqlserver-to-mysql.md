---
title: SqlServer 迁移到mysql
toc: true
top: 100
mathjax: false
date: 2018-12-25 21:56:21
tags:
- mysql
- sqlserver

categories:
- 代码世界
- 数据库迁移
---
# 序
最近有开展Sqlserver 迁移到mysql的工作,以下经验基本上是别的博客没有提到过的，故行此文以作补充。

# 版本差异
## 时间精度
Mysql 5.5版本不支持毫秒,Mysql 5.6以上才支持。
时间格式为**DateTime(3)**,当前时间为**Now(3)**。在5.5版本上,这两种写法为提示错误。

## row_number函数
mysql 8.0才支持row_number函数，低版本都不支持该函数。
**低版本实现方式:** 游标查询,或者少量数据的情况下,创建带有自增主键的临时表。

## mysql.proc表
该表在mysql 8.0版本中已经不存在了,所以 **<font color=#FF0000>select * from mysql.proc；</font>**在mysql 8.0版本中会报错;  
需要转换成 **<font color=#FF0000>show procedure status where db='sys';</font>**这种写法来获取存储过程的相关信息。

## with cte
mysql 8.0以上才支持cte,不过[Mysql递归cte](https://dev.mysql.com/doc/refman/8.0/en/with.html#common-table-expressions-recursive)的写法与sql server存在差异,需要添加关键字**recursive**,否则会提示cte不存在错误。

如下
```
with recursive cte as
(
    select Id,Pid,DeptName,0 as lvl from Department
    where Id = 2
    union all
    select d.Id,d.Pid,d.DeptName,lvl+1 from cte c inner join Department d
    on c.Id = d.Pid
)
select * from cte
```

## .net framework框架

5\.6 及以上的的版本使用**mysql\.data\.dll**时需要 **\.net framework 4\.5\.2**以上。

# Mysql不具备的SQL Server语法

## uniqueidentifier
SqlServer 通过WorkBench 迁移到Mysql时，该字段会自动变成varchar(36)类型。Mysql对应的生成Guid的语法是**uuid()**。  
因object不支持强制转换成Guid，所以应将接口代码中的 **(Guid)cmd.ExecuteScalar()**替换成**new Guid(cmd.ExecuteScalar())**

## deleted,Inserted
SqlServer在执行 insert into table_a select top 10 columnname from table_b时可以通过inserted获取到新插入的十条记录的自增主键,而Mysql不可以。  
**实现方式**  表新增Guid辅助字段,并将Guid放在临时表,通过多表链接查询获取数据。

## select top ... percent
SqlServer在执行 select top 10 percent columnname from table_a,如果table_a中有200条记录,则会返回前20条记录。  
**实现方式**  Mysql 需要通过获取总记录条数，然后获取总条数*百分比来获取数据。

# 临时表编码

CREATE TEMPORARY TABLE  
temptable1(groupname VARCHAR(50))  
 **<font color=#FF0000>DEFAULT CHARSET=utf8;</font>**
这样可以解决临时表不能插入中文的问题。

# 配置文件

## 默认位置
各版本的配置文件的位置如下:  
**5.5：** C:\\<font color=#FF0000>Program Files</font>\MySQL\MySQL Server 5.5\my.ini  
**5.6：** C:\\<font color=#FF0000>ProgramData</font>\MySQL\MySQL Server 5.6\my.ini  
**5.7：** C:\\<font color=#FF0000>ProgramData</font>\MySQL\MySQL Server 5.7\my.ini  
**8.0：** C:\\<font color=#FF0000>ProgramData</font>\MySQL\MySQL Server 8.0\my.ini   
**<font color=#FF0000>配置文件修改以后重启mysqlserver才会生效。</font>**

## longtext存储
max_allowed_packet 的默认值是4M。  
设置**max_allowed_packet=100M**可以解决longtext数据的存储长度的问题。

## 数据库名大小写
lower_case_table_names
设置lower_case_table_names=1,表名称都设置为统一小写。

## id自增不连续
innodb_autoinc_lock_mode=0  解决id自增不连续的问题；

# 语法差异

## 不等于
SqlServer的不等于符号 **!=**中间可以存在空格，而Mysql的不可以。
Sql Server： select \* from a where a.column !    = 0  <font color=#FF0000>不报错</font>  
Mysql： select \* from a where a.column !    = 0       <font color=#FF0000>报错</font>  
Mysql： select \* from a where a.column != 0           <font color=#FF0000>不报错</font>  

## 逻辑取反
SqlServer ： update table_a set isOk=~isOk;
Mysql     ： update table_a set isOk=!isOk;

# 总结
以上就是本人在做SqlServer 迁移到Mysql的过程中遇到的问题以及相应的解决方案。  
如果其中有不正确的地方，强烈欢迎指正，避免误导他人。  
希望该文能为你的迁移大业贡献一份力量。