_**记录做过的具有代表性的MySQL算法题，可快速阅览回顾SQL语法和一些操作，题目均来源网上资料，具体可参考力扣、牛客**_

**记录格式：**
    
 ·标题  ·题目简介 ·示例  ·思路简介  ·实现代码

----
**目录：**
    
    
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

