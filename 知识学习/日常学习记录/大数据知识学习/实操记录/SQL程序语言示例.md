_____**本文记录SQL程序语言的一些语句操作，代码示例，通过此文可以快速回忆SQL的写法，摘抄于网上资料，来源链接见文末**_


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

----

<font color= #871F78>****MySQL****</font>


**1、SHOW**

```mysql
# (1)查询所有数据库:
    SHOW DATABASES;

# (2)查询所有数据表:
    SHOW TABLES;

# (3)查询数据表的字符集:
    SHOW TABLE STATUS FROM mysql LIKE 'user';
# 语法：SHOW TABLE STATUS FROM 数据库名称 LIKE '表名';

# (4)查询数据库的创建表语句:
    SHOW CREATE DATABASE mysql;
# 语法：SHOW CREATE DATABASE 数据库名称;

# (5) 查看表状态(引擎):
    SHOW table status;
    SHOW create table tab_name;
```
   
    

**2、CREATE**
    
```mysql
# (1)创建数据库:
    CREATE DATABASE /* [ IF NOT EXISTS] */ db1;

# (2)创建数据库（指定字符集）:
    CREATE DATABASE db3 CHARACTER SET utf8;

# (3)创建数据表:
    CREATE TABLE product(
            id INT,
            NAME VARCHAR(20),
            price DOUBLE,
            stock INT
    );
    /*语法：CREATE TABLE [if not exists] 表名(
                    列名 数据类型 约束(默认值、唯一、非空),
                    列名 数据类型 约束,
                    ...
                    列名 数据类型 约束,
            );
    */

# (4)创建外键:
    create table student(
        stuId int(11) auto_increment primary key,
        stuName varchar(50),
        gradeId int(4),
        phone varchar(4),
        constraint `s_g_key` foreign  key (gradeId) references grade (GradeId)
    );

# (5)创建索引：
    Create index idx_age on person(age);
    Create unique index idx_name on person(name);
# 语法：Create [unique] index index_name on table_name(column_name);
# unique表示该索引是否唯一，index_name表示索引名称，table_name表示要创建索引的表名称，column_name表示要创建索引的列名。

# (6)创建视图:
    Create view v_person as select id, name, age from person;
# 语法：Create [algorithm = {undefined | merge | temptable}] view view_name as select_statement;
# 其中，algorithm表示创建视图使用的算法，默认为undefined，view_name表示要创建的视图名称，select_statement表示用于创建视图的SELECT查询语句。

```

**3、ALTER**

```mysql
# (1)删除字段:
    ALTER TABLE student  DROP age;
    ALTER TABLE student  MODIFY age CHAR(100);
    ALTER TABLE student CHANGE id  stu_id BIGINT PRIMARY KEY;  #在 CHANGE 关键字之后，紧跟要修改的字段名，然后指定新字段的类型及名称
    ALTER TABLE student ALTER sex DROP DEFAULT;  #使用 ALTER 命令及 DROP子句来删除字段的默认值

# (2)添加字段：
    ALTER TABLE tab_name ADD field type (first/before field1);

# (3)修改字段：
    # 修改字段类型(修改字段相对位置)   [modify]
    alter table tab_name modify field type (first/before field1);

    # 修改字段默认值/是否为空/自动增长
    alter table tab_name modify field type not null/default ="未知"/auto_increment;

    # 修改字段名/字段类型!   [change]
    alter table tab_name change field  newfield newtype;

# (4)修改表名:
    ALTER TABLE student RENAME TO student_1;

# (5)添加与删除主键：
    alter table tab_name add primary key(field);
    alter table tab_name drop primary key;

# (6)添加与删除外键约束:
    # 给tab_name 中的 field列添加名为fk_name的约束绑定 x表中的主键i
    alter table tab_name add constraint fk_name foreign key(field) references x(i);

    alter table tab_name drop foreign key fk_name;
    alter table tab_name drop index  index_name;

# (7) 设置约束与默认值:
    # 设置唯一约束
    alter table tab_name add constraint uq_name unique(field);
    
    # 删除唯一约束
    alter table tab_name drop index uq_name;

    # 设置默认值
    alter table tab_name field set default "默认";
    
    # 删除默认值
    alter table tab_name field drop default;

# (8) 修改数据库表存储引擎:
    alter table altertable engine = myisam;

```

**4、DROP**

```mysql
# (1) 删除数据库：
    DROP DATABASE [IF EXISTS] database_name;
    
# (2) 删除数据表及其数据和表结构：
    DROP TABLE [IF EXISTS] table_name;
    
# (3) 删除视图：
    DROP VIEW [IF EXISTS] view_name;
    
# (4) 删除函数：
    DROP FUNCTION [IF EXISTS] function_name;
    
# (5) 删除存储过程：
    DROP PROCEDURE [IF EXISTS] procedure_name;

# (6) 删除触发器:
    DROP TRIGGER [IF EXISTS] trigger_name;

```

**5、TRUNCATE**

```mysql
# 清空表，不删除表结构L:
    TRUNCATE TABLE TABLENAME #或者
    TRUNCATE TABLENAME
```

##2.2、DQL（数据查询语言）

**1、SELECT**

```mysql
# (1) 基本查询语句
    SELECT '任何值'; #仅限mysql
    SELECT * FROM TABLE;
    SELECT a,b FROM TABLE;
    SELECT A AS '别名' FROM TABLE;

# (2) DISTINCT去重：
    SELECT DISTINCT department_id FROM employees;
    
# (3) 全套关键字查询语句：
    SELECT
        *
    FROM TABLE
    WHERE A = '1'
    GROUP BY A
    HAVING COUNT(*) >= 3
    ORDER BY A ASC
    LIMIT 100;
    
# (4) SELECT的查询还有多表查询、单行函数使用、聚合函数的使用、窗口函数查询、子查询、关联查询等等--后续再补充
```

##2.3、DML（数据控制语言）

**1、INSERT**

```mysql
# (1) 插入数据，可单行可多行：
	INSERT INTO TABLE_NAME [(value1, value2, value3, ...)] VALUES (data1, data2, data3, ...),(data1, data2, data3, ...) ...
	
	/*
		注意:

			1、表的字段是可选的，入锅省略，则依次插入所有的字段。

			2、如果插入的是表中部分列的数据，字段名列表必须填写。

			3、多个字段和多个值之间使用逗号隔开。

			4、值列表必须和字段名列表数量相同且数据类型相符（字符串和日期类型的值要加单引号）。

			5、值列表中的数据必须符合数据完整性的要求。
	*/

# (2) INSERT IGNORE INTO
	INSERT IGNORE INTO TABLE_NAME [(value1, value2, value3, ...)] VALUES (data1, data2, data3, ...),(data1, data2, data3, ...) ...
	#当插入新数据时，MySQL数据库会首先检索已有数据，如果存在，则忽略本次插入，如果不存在，则插入
	
# (3) ON DUPLICATE KEY UPDATE
	INSERT INTO person(pName,id_card,salary,summary) VALUES ('张无忌', '110101199003077417', '888.99', '武学大才') on duplicate key update salary=3333,summary='武学巅峰';
    #插入数据时，如果数据存在，则执行on update后更新操作，如果不存在，则直接插入，注意更新的数据是 on duplicate key update 关键字后面的字段。

# (4) REPLACE INTO :
    REPLACE INTO TABLE_NAME [(value1, value2, value3, ...)] VALUES (data1, data2, data3, ...)
    #	插入数据时，如果数据存在，则删除后再插入，如果不存在，则直接插入
```

**2、UPDATE**

```mysql
# (1) 更新字段值
    UPDATE table SET column1 = value1, column2 = value2, ... WHERE condition;
    #注意：更新表中的记录时要小心！ 注意 UPDATE 语句中的 WHERE 子句。 WHERE 子句指定应该更新哪些记录。 如果省略WHERE子句，表中的所有记录都会更新！
    
# (2) mysql数据库update更新表中某个字段的值为另一张表的某个字段值  
    UPDATE table1 a LEFT JOIN table2 b ON a.idd= b.idd SET a.val = b.val WHERE a.idd=b.idd;

# (3) mysql查询出一张表的数据值去更新另一张表的数据值
    UPDATE table1 SET val=(SELECT val FROM table2 WHERE idd=‘01’) WHERE idd=‘03’

# (4) 对某些字段变量+1，常见的如：点击率、下载次数等这种直接将字段+1然后赋值给自身
    UPDATE table1 SET val=val+1 WHERE idd=‘06’

```

##2.4、TCL（事务控制语言）

```mysql
# (1) 开启事务
    begin;
    #或 
    start transaction;
    
# (2) 事务提交
    commit [work];
    
# (3) 回滚
    rollback [work];
    
# (4) 保存点设置
    savepoint 标识;

# (5) 回滚到保存点
    rollback to [savepoint] 标识;
    
# (6) 删除保存点
    release savepoint 标识;

```

# 示例
```mysql
    begin;

    insert into testtcl values(47, 's');

    savepoint a;

    insert into testtcl values(48, 's');
    
    savepoint b;
    
    insert into testtcl values(49, 's');
    
    rollback to a;
```

##2.5、DCL（数据控制语言）

```mysql
# (1) 创建用户
    create user '用户名'@'主机名' identified by '密码';

# (2) 修改用户密码
    alter user '用户名'@'主机名' identified with mysql_native_password by '新密码';

# (3) 修改用户密码
    alter user '用户名'@'主机名' identified with mysql_native_password by '新密码';

# (4) 删除用户 
    drop user '用户名'@'主机名';

# (5) 查询权限
    show grants for '用户名'@'主机名';

# (6) 授予权限 
    grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';
```

常用的权限有以下几种：

| 权限                   | 说明             | 
|----------------------|----------------|
| ALL, ALL PRIVILEGES  | 所有权限           |
| SELECT               | 查询数据           |
| INSERT               | 插入数据           |
| UPDATE               | 修改数据           | 
| DELETE               | 删除数据           | 
| ALTER                | 修改表            |
| DROP                 | 删除数据库 / 表 / 视图 |
| CREATE               | 创建数据库/表        |


#3、参考链接

[1] CSDN：SQL语言的分类(https://blog.csdn.net/zhanghuiqi205/article/details/124930881)

[2] CSDN：MySQL基础之DDL命令(https://blog.csdn.net/m0_43405302/article/details/121677165)

[3] 百度：MySQL的create语句详解(https://baijiahao.baidu.com/s?id=1760939803942190781&wfr=spider&for=pc)

[4] CSDN: Mysql中的alter命令大全(https://blog.csdn.net/weixin_52345071/article/details/129815543)

[5] HTML: MySQL Drop操作详解(https://www.python100.com/html/P912URT0E78N.html)

[6] CSDN: mysql基本语句：DML（数据操作语言）(https://blog.csdn.net/qi341500/article/details/126594024)

[7] CSDN: TCL——事务控制语言(https://blog.csdn.net/qq_40678222/article/details/113777410)

[8] CSDN: DCL——数据控制语言(https://blog.csdn.net/Inneverleft/article/details/127171303)

