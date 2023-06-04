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

```

**实现代码：**

```sql


```

----

