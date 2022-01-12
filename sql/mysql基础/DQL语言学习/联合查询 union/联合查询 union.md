# 联合查询 union

关键字：union   将多条查询语句的结果合并成一个结果

### 语法

```SQL
select 字段|常量|表达式|函数 【from 表】 【where 条件】 union 【all】
select 字段|常量|表达式|函数 【from 表】 【where 条件】 union 【all】
select 字段|常量|表达式|函数 【from 表】 【where 条件】 union  【all】
.....
select 字段|常量|表达式|函数 【from 表】 【where 条件】
```


### 特点

```SQL
1、多条查询语句的查询的列数必须是一致的
2、多条查询语句的查询的列的类型几乎相同
3、union代表去重，union all代表不去重
```


```SQL
# 查询部门编号 >90 ，或者是 邮箱中包含 a 员工信息
select * from employees where email like '%a%' or department_id  > 90;


select * from employees where email like '%a%'
union
select * from employees where department_id > 90;

```


### 应用场景

```SQL
要查询的结果来自于多个表，且多个表没有直接的连接关系，但查询的信息一致时，就使用 union
来查询

# eg: 例如查询中国男性和外国男性的信息
select id,name,csex from t_ca where csex = '男'
union 
select t_id,tName,from t_ua where tGender = 'male';


```


