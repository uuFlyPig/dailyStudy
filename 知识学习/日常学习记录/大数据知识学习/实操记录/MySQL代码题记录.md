_**记录做过的具有代表性的MySQL算法题，可快速阅览回顾SQL语法和一些操作，题目均来源网上资料，具体可参考力扣、牛客**_

**记录格式：**
    
 ·标题  ·题目简介 ·示例  ·思路简介  ·实现代码

----
**目录：**
    [1、计算特殊金额](1、计算特殊金额)
    



####1、计算特殊金额

**题目简介：**

```sql
写出一个SQL查询语句，计算每个雇员的奖金。如果一个雇员的id是奇数并且他的名字不是以'M'开头，那么他的奖金是他工资的100%，否则奖金为0。
```

**示例：**

<font color = green>Employees表</font>

| employee_id | name    | salary |
|-------------|---------|--------|
| 2           | Meir    | 3000   |
| 3           | Michael | 3800   |
| 7           | Addilyn | 7400   |
| 8           | Juan    | 6100   |
| 9           | Kannon  | 7700   |

<font color= #871F78>输出：<font>

| employee_id | bonus |
|-------------|-------|
| 2           | 0     |
| 3           | 0     |
| 7           | 7400  |
| 8           | 0     |
| 9           | 7700  |

**思路简介：**

```sql
    题目要求是判断雇员的信息，之后返回工资，因此按照id为奇数，可以使用余数为1进行判断，不是偶就是奇mod(employee_id,2) != 0;
        且名字不以'M'开头，因此left(name,1) != 'M%',bonus为1*bonus，否则为0.
        最后不要忘了按照employee_id排序。
```

**实现代码：**

```sql
    select 
        employee_id,
        if(mod(employee_id,2) != 0 and left(name,1) != 'M%',bonus,0)
    from Employees 
    order by employee_id
```

----

####2、计算25岁以上和以下的用户数量

**题目简介：**

```sql
现在运营想要将用户划分为25岁以下和25岁及以上两个年龄段，分别查看这两个年龄段用户数量
```

**示例：**

 输入：<font color = green>Employees表</font>

| id  | device_id | gender | age | university | gpa | active_days_within_30 | question_cnt | answer_cnt |
|-----|-----------|--------|-----|------------|-----|-----------------------|--------------|------------|
| 1   | 2138      | male   | 21  | 北京大学       | 3.4 | 7                     | 2            | 12         |
| 2   | 3214      | male   |     | 复旦大学       | 4.0 | 15                    | 5            | 25         |
| 3   | 6543      | female | 20  | 北京大学       | 3.2 | 12                    | 3            | 30         |
| 4   | 2315      | female | 23  | 浙江大学       | 3.6 | 5                     | 1            | 2          |
| 5   | 5432      | male   | 25  | 山东大学       | 3.8 | 20                    | 15           | 70         |
| 6   | 2131      | male   | 28  | 山东大学       | 3.3 | 15                    | 7            | 13         |
| 7   | 4321      | male   | 26  | 复旦大学       | 3.6 | 9                     | 6            | 52         |


<font color= #871F78>输出：<font>

| age_cut | number |
|---------|--------|
| 25岁以下   | 4      |
| 25岁及以上  | 3      |

**思路简介：**

```sql
统计25岁以下，25岁及以上的员工人数，主要使用分组统计行数的方式，case when（if也可） + group by ；另一种方式可以使用分别筛选出年龄count后union
    25岁以及年龄未知的分为25岁以下，因此(age < 25 or age is null)  '25岁以下'
```

**实现代码：**

```sql
    select 
        if(age <25 or age is null, '25岁以下', '25岁及以上') as age_cut,
        --或用case when then 
        count(*) as number
    from Employees
    group by age_cut
    
    
```

----

####3、修复表中的字母

**题目简介：**

```sql
编写一个 SQL 查询来修复名字，使得只有第一个字符是大写的，其余都是小写的。
返回按 user_id 排序的结果表。
```

**示例：**

输入：无

<font color= #871F78>输出：<font>

**思路简介：**

```sql
通过字符串函数，对字符串进行截取转换后再拼接，实现将某一字母变成大写的目的
concat字符串拼接函数，substr字符串截断函数，upper，lower字符大小写函数
```

**实现代码：**

```sql
select 
    user_id,
    concat(upper(left(name,1)),lower(substr(name,2))) as name
from Users
order by user_id
```

----

####4、按日期分组销售产品

**题目简介：**

```sql
查找每个日期、销售的不同产品的数量及其名称。每个日期的销售产品名称应按词典序排列。返回按 sell_date 排序的结果表。
```

**示例：**

输入：<font color = green>Activities 表：</font>

| sell_date  | product    |
|------------|------------|
| 2020-05-30 | Headphone  |
| 2020-06-01 | Pencil     |
| 2020-06-02 | Mask       |
| 2020-05-30 | Basketball |
| 2020-06-01 | Bible      |
| 2020-06-02 | Mask       |
| 2020-05-30 | T-Shirt    |

<font color= #871F78>输出：<font>

| sell_date  | num_sold | products                     |
|------------|----------|------------------------------|
| 2020-05-30 | 3        | Basketball,Headphone,T-shirt |
| 2020-06-01 | 2        | Bible,Pencil                 |
| 2020-06-02 | 1        | Mask                         |


**思路简介：**

```sql
每个日期是以日期为分组，不同的产品数量即需要对产品进行去重后统计count(distinct product),根据输出是分组后对字段进行拼接
使用函数group_concat，在函数内部进行排序指定分隔符等操作。

    --将分组中column1这一列对应的多行的值按照column2 升序或者降序进行连接，其中分隔符为seq
    --如果用到了DISTINCT，将表示将不重复的column1按照column2升序或者降序连接
    --如果没有指定SEPARATOR的话，也就是说没有写，那么就会默认以 ','分隔
    GROUP_CONCAT([DISTINCT] column1 [ORDER BY column2 ASC\DESC] [SEPARATOR seq]);
```

**实现代码：**

```sql
    select 
        sell_date,
        count(distinct product) as num_sold,
        group_concat(distinct product order by product separator ',') as products
    from Activities
    group by sell_date
    order by sell_date
```

----

####5、丢失的雇员信息

**题目简介：**

```sql
写出一个查询语句，找到所有 丢失信息 的雇员id。当满足下面一个条件时，就被认为是雇员的信息丢失：
雇员的 姓名 丢失了，或者
雇员的 薪水信息 丢失了
```

**示例：**

输入：<font color = green>Employees表</font>

| employee_id | name     | 
|-------------|----------|
| 2           | Crew     | 
| 4           | Haven    | 
| 5           | Kristian |

<font color = green>Salaries表</font>

| employee_id | salary |
|-------------|--------|
| 5           | 76071  |
| 1           | 22517  |
| 4           | 63539  |

<font color= #871F78>输出：<font>

| employee_id | 
|-------------|
| 1           |
| 2           |


**思路简介：**

```sql
将雇员表进行连接，连接后雇员姓名为空或雇员薪水为空即为丢失信息的雇员，方法上可以将二表进行全外联后进行条件判断
但mysql中没有全外联，因此可以考虑先内连接取出信息不丢失的雇员，再排除出丢失信息的雇员
或使用union，将or的条件拆分，union进行连接。雇员表中id均有名字，薪水表中雇员均有薪水，即挑选出雇员在薪水表中没有薪水的，在雇员表中没有存在的union即可
```

**实现代码：**

```sql
select employee_id from Employees where employee_id not in (select employee_id from Salaries)
union
select employee_id from Salaries where employee_id not in (select employee_id from Employees)
order by employee_id 
```

----

####6、表格转换

**题目简介：**

```sql
将多列的表格转换成多行即（列转行）。
```

**示例：**

输入：<font color = green>Products表</font>

| product_id | store1 | store2 | store3 |
|------------|--------|--------|--------|
| 0          | 95     | 100    | 105    |
| 1          | 70     | null   | 80     |


<font color= #871F78>输出：<font>

| product_id | store  | price |
|------------|--------|-------|
| 0          | store1 | 95    |
| 0          | store2 | 100   |
| 0          | store3 | 105   |
| 1          | store1 | 70    |
| 1          | store3 | 80    |

**思路简介：**

```sql
列转行使用union all或者union，将数据行分别select 再union，最后注意值为空是否输出以及是否需要排序。
行转列则可以使用group by + sum的搭配。
```

**实现代码：**

```sql
select product_id,'store1' as store,store1 as price where store1 is not null 
union all
select product_id,'store2' as store,store2 as price where store2 is not null
union all
select product_id,'store3' as store,store3 as price where store3 is not null
```

----

####7、树节点

**题目简介：**

```sql
给定一个表tree，id是树节点的编号，p_id是它父节点的编号，树中每个节点属于以下三种类型之一：
    ·叶子：如果这个节点没有任何孩子节点。
    ·根：如果这个节点是整棵树的根，即没有父节点
    ·内部节点：如果这个节点既不是叶子节点也不是根节点。
输出节点id，以及其对应的节点类型。
```

**示例：**

输入：<font color = green>tree表</font>

| id  | p_id | 
|-----|------|
| 1   | null |
| 2   | 1    | 
| 3   | 1    |
| 4   | 2    |
| 5   | 2    |

<font color= #871F78>输出：<font>

| id  | Type  |
|-----|-------|
| 1   | root  |
| 2   | Inner |
| 3   | Leaf  |
| 4   | Leaf  |
| 5   | Leaf  |

**思路简介：**

```sql
根据题目，当id的父节点p_id为null时即此id为根节点root，当id不存在子节点的时候即id not in(父节点集合)为叶子节点，其余为中间节点
```

**实现代码：**

```sql
    select
        id,
        case when p_id is null then 'root'
            when id not in (select p_id from tree where p_id is not null) then 'leaf'
            else 'inner'
        end as type 
    from tree
    
    --为什么要在where中添加is not null，因为如果判断中not in（A），A中含有一个null就会全部返回null，不管后面是否符合
```

----

####8、查询第二高薪水

**题目简介：**

```sql
查找薪资表中的第二高薪水,如果没有第二高的薪水则输出null
```

**示例：**

输入：<font color = green>Employee表</font>     

| id  | salary |              
|-----|--------|
| 1   | 100    |
| 2   | 200    |
| 3   | 300    |


<font color= #871F78>输出：<font>

| SecondHighestSalary |
|---------------------|
| 200                 |

输入：<font color = green>Employee表</font>

| id  | salary |              
|-----|--------|
| 1   | 100    |

<font color= #871F78>输出：<font>

| SecondHighestSalary |
|---------------------|
| null                |

**思路简介：**

```sql
在mysql中利用排序后，limit将处于第二高的薪水取出，limit 1,1，需要注意薪水去重以及null的输出
```

**实现代码：**

```sql
    select
           ifnull(
               (select distinct salary 
               from Employee 
               order by salary desc 
                   limit 1,1
               ),null) AS SecondHighestSalary

```

----

####9、上升温度

**题目简介：**

```sql
今天比昨天的温度高，且天数相差1，求满足这种情况的今天的id
```

**示例：**

输入：<font color = green>Weather表</font>

| id  | recordDate | Temperature |
|-----|------------|-------------|
| 1   | 2015-01-01 | 10          |
| 2   | 2015-01-02 | 25          |
| 3   | 2015-01-03 | 20          |
| 4   | 2015-01-04 | 30          |

<font color= #871F78>输出：<font>

| id  | 
|-----|
| 2   |
| 4   |


**思路简介：**

```sql
通过自连接join，将日期相差1 datediff(t1.day,t2.day) = 1 以及温度大小条件t1.t > t2.table 
设置在连接条件中，即可以把数据筛选出来。
```

**实现代码：**

```sql
    select 
        t2.id
    from Weather as t1 join Weather as t2 
        on datediff(t2.recordDate-t1.recordDate) = 1
        and t2.Temperature > t1.Temperature

```

----

####10、求最大值，求一类的最大值

**题目简介：**

```sql
取出下列订单表中，哪个顾客的订单数是最多的，取订单数最大的顾客number。
```

**示例：**

输入：<font color = green>Orders表</font>

| order_number | customer_number |
|--------------|-----------------|
| 1            | 1               | 
| 2            | 2               |
| 3            | 3               |
| 4            | 3               |


<font color= #871F78>输出：<font>

| customer_number |
|-----------------|
| 3               |

**思路简介：**

```sql
取订单数最大的顾客，以顾客的number为分组条件，count(order_number)统计订单数,having中嵌套子查询> ALL()取最大值
```

**实现代码：**

```sql
select customer_number
from Orders
group by customer_number
having count(*) >= ALL (
    select count(order_number)
    from Orders
    group by customer_number
)

--窗口函数实现
select t.customer_number
from
    (select customer_number, rank() over(order by count(order_number) desc) as ranking
     from orders
     group by customer_number) t
where t.ranking = 1
```

----

####11、求各个玩家的日期第一个

**题目简介：**

```sql
写一条SQL查询语句获取每位玩家第一次登录平台的日期
```

**示例：**

输入：<font color = green>Activity表</font>

| player_id | device_id | event_date | games_played |
|-----------|-----------|------------|--------------|
| 1         | 2         | 2016-03-01 | 5            |
| 1         | 2         | 2016-05-02 | 6            |
| 2         | 3         | 2017-06-25 | 1            | 
| 3         | 1         | 2016-03-02 | 0            |
| 3         | 4         | 2018-07-03 | 5            |

<font color= #871F78>输出：<font>

| player_id | first_loin |
|-----------|------------|
| 1         | 2016-03-01 |
| 2         | 2017-06-25 |
| 3         | 2016-03-02 |

**思路简介：**

```sql
两种实现方式 ,首先可以通过group by + min函数的方式选取用户最小的登录日期，即为第一次登录的日期。
            其次可以通过窗口函数实现取得第一次登录的日期。
```

**实现代码：**

```sql
--普通实现
select 
    player_id,
    min(event_date) as first_loin
from Activity
group by player_id


--窗口函数
select
    distinct player_id, min(event_date) over(partition by player_id) as first_login
from Activity
    
```

----

####12、2020年最后一次登录

**题目简介：**

```sql
查询可以获取在 2020 年登录过的所有用户的本年度 最后一次 登录时间。结果集不包含 2020 年没有登录过的用户。
```

**示例：**

输入：<font color = green>Logins表</font>

| user_id | time_stamp            | 
|---------|-----------------------|
| 6       | 2020-06-30 15:06:07   |
| 6       | 2021-04-21 14:06:06   | 
| 6       | 2019-03-07 00:18:15   | 
| 8       | 2020-02-01 05:10:53   |   
| 8       | 2020-12-30 00:46:50   |
| 2       | 2020-01-16 02:49:50   |
| 2       | 2019-08-25 07:59:08   |
| 14      | 2019-07-14 09:00:00   |
| 14      | 2021-01-06 11:59:59   | 

<font color= #871F78>输出：<font>

| user_id | last_stamp          |
|---------|---------------------|
| 6       | 2020-06-30 15:06:07 |
| 8       | 2020-12-30 00:46:50 |
| 2       | 2020-01-16 02:49:50 |


**思路简介：**

```sql
获取2020年登录过的，2020未登录不用选择则可以在where条件中筛选出2020年的登录信息，
再按照id  group by + max取出最大的日期即为最后一次登录时间。
```

**实现代码：**

```sql

    select
        user_id,max(time_stamp) as last_stamp
    from Logins
    where year (time_stamp) = 2020
    group by user_id
    
```

----

####13、股票的资本损益

**题目简介：**

```sql
求买入和卖出的差值，即为收益多少
```

**示例：**

输入：<font color = green>Stocks表</font>

| stock_name   | operation | operation_day | price |
|--------------|-----------|---------------|-------|
| Leetcode     | Buy       | 1             | 1000  |
| Corona Masks | Buy       | 2             | 10    |
| Leetcode     | Sell      | 5             | 9000  |
| Handbags     | Buy       | 17            | 30000 |
| Corona Masks | Sell      | 3             | 1010  |
| Corona Masks | Buy       | 4             | 1000  |
| Corona Masks | Sell      | 5             | 500   |
| Corona Masks | Buy       | 6             | 1000  |
| Handbags     | Sell      | 29            | 7000  |
| Corona Masks | Sell      | 10            | 10000 |

<font color= #871F78>输出：<font>

| stock_name   | capital_gain_loss |
|--------------|-------------------|
| Corona Masks | 9500              |
| Leetcode     | 8000              |
| Handbags     | -23000            |

**思路简介：**

```sql
利用group by + sum，sum里面再嵌套if判断如果是Buy则价格为负，否则为正，进行合计计算即可。
```

**实现代码：**

```sql
    select
        stock_name,
        sum(if(operation = "Buy",-price,price)) as capital_gain_loss
    from Stocks
    group by stock_name
```

----

####14、市场分析

**题目简介：**

```sql
请写出一条SQL语句以查询每个用户的注册日期和在2019年作为买家的订单总数
```

**示例：**

输入：<font color = green>Users表</font>

| user_id | join_date  | favorite_brand |
|---------|------------|----------------|
| 1       | 2018-01-01 | Lenovo         |
| 2       | 2018-02-09 | Samsung        |
| 3       | 2018-01-19 | LG             |
| 4       | 2018-06-21 | HP             |

<font color = green>Orders表</font>

| order_id | order_date | item_id | buyer_id | seller_id |
|----------|------------|---------|----------|-----------|
| 1        | 2019-08-01 | 4       | 1        | 2         |
| 2        | 2018-08-02 | 2       | 1        | 3         |
| 3        | 2019-08-03 | 3       | 2        | 3         |
| 4        | 2018-08-04 | 1       | 4        | 2         |
| 5        | 2018-08-04 | 1       | 3        | 4         |
| 6        | 2019-08-05 | 2       | 2        | 4         |

<font color = green>Items表</font>

| item_id | item_brand | 
|---------|------------|
| 1       | Samsung    |
| 2       | Lenovo     | 
| 3       | LG         | 
| 4       | HP         |


<font color= #871F78>输出：<font>

| buyer_id | join_date  | orders_in_2019 |
|----------|------------|----------------|
| 1        | 2018-01-01 | 1              |
| 2        | 2018-02-09 | 2              |
| 3        | 2018-01-19 | 0              |
| 4        | 2018-05-21 | 0              |

**思路简介：**

```sql
计算用户在2019年作为买家的订单总数，在订单表中选取出2019的订单，以buyer_id，买家为分组条件，统计总数，并且将Users关联取得用户注册日期
```

**实现代码：**

```sql
select
    buyer_id,
    join_date,
    count(*)
from Orders AS A left join Users AS B on A.buyer_id = B.user_id 
where year(order_date) = '2019'
group by buyer_id

```

----

####15、连续出现的数字

**题目简介：**

```sql
编写一个 SQL 查询，查找所有至少连续出现三次的数字。
返回的结果表中的数据可以按 任意顺序 排列。
```

**示例：**

输入：<font color = green>Logs表</font>

| Id  | Num | 
|-----|-----|
| 1   | 1   | 
| 2   | 1   | 
| 3   | 1   |
| 4   | 2   |
| 5   | 1   |
| 6   | 2   |
| 7   | 2   |



<font color= #871F78>输出：<font>

| ConsecutiveNums | 
|-----------------|
| 1               | 


**思路简介：**

```sql

```

**实现代码：**

```sql


```

----

