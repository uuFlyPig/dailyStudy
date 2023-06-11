_**本文记录SQL程序语言的一些语句操作，代码示例，通过此文可以快速回忆SQL的写法，摘抄于网上资料，来源链接见文末**_


#1、SQL语言程序简介

结构化查询语言(Structured Query Language)简称SQL。

SQL语言通常分为五类：

    DDL(数据定义语言)
        简介：数据定义语言（Data Definition Language）简称DDL。是 SQL 语言集中负责数据结构定义与数据库对象定义的语言。
        核心指令：create、alter、drop等；
        作用：DDL用来创建数据库中的各种对象，创建、删除、修改表的结构，比如表、视图、索引、同义词、聚簇等。其主要功能是定义数据库对象

    DQL(数据查询语言)
        简介：数据查询语言（Data Query Language）简称DQL。是SQL语言中，负责进行数据查询而不会对数据本身进行修改的语句，这是最基本的SQL语句。
        核心指令：select等；
        作用：通常与关键字from、where、group by、having、order by等一起使用，组成查询语句。

    DML(数据操纵语言)
        简介：数据操纵语言（Data Manipulation Language）简称DML。过它可以实现对数据库的基本操作，对数据库其中的对象和数据运行访问工作的语言。
        核心指令：insert、delete 、update等；
        作用：DML的主要功能是访问数据，因此其语法都是以读写数据库为主。DML是修改数据库表中的数据，而 DDL 是修改数据中表的结构。

    TCL(事务控制语言)
        简介：事务控制语言（Transaction Control Language）简称TCL。用于管理数据库中的事务。这些用于管理由 DML 语句所做的更改。它还允许将语句分组为逻辑事务。
        核心指令：commit、rollback等；        
        作用：TCL经常被用于快速原型开发、脚本编程、GUI和测试等方面。

    DCL(数据控制语言)
        简介：数据控制语言 （Data Control Language）简称DCL。是一种可对数据访问权进行控制的指令，它可以控制特定用户账户对数据表、查看表、预存程序、用户自定义函数等数据库对象的控制权。
        核心指令：grant、revoke等。
        作用：DCL用来授予或回收访问数据库的某种特权，并控制数据库操纵事务发生的时间及效果，对数据库实行监视等。

    
#2、SQL实例

##2.1、DDL（数据定义语言）


##2.2、DQL（数据查询语言）


##2.3、DML（数据控制语言）


##2.4、TCL（事务控制语言）


##2.5、DCL（数据控制语言）



#3、参考链接

[1] CSDN：SQL语言的分类（https://blog.csdn.net/zhanghuiqi205/article/details/124930881）
