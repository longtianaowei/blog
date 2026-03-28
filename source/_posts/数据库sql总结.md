---
title: 数据库sql总结
date: 2024-07-09 14:53:38
tags:
---

# 数据库SQL总结

## 1、DDL 数据库定义语言

- ###  对数据库的操作

  #### 使用数据库

  ```sql
  use 数据库名;
  ```

  #### 查

  查询所有数据库：

  ```sql
  show databases;
  ```

  查询当前数据库：

  ```sql
  select database();
  ```

  #### 增

  ```sql
  create database [if not exists] 数据库名；
  ```

  如：create database test;  #创建一个test数据库

  #### 删

  ```sql
  drop database [if exists] 数据库名;
  ```

  如:drop database test; #删除test数据库

  

  PS：database可替换为schema

  如：create schema test;



- ## 对表的操作

  #### 增

  ```sql
  create table 表名(
  字段1 字段1类型 [约束] [comment 字段1注释 ], 
  字段2 字段2类型 [约束] [comment 字段2注释 ], 
  ......
  字段n 字段n类型 [约束] [comment 字段n注释 ]
  ) [ comment 表注释 ];
  ```

  如：

  ```sql
  create table tb_user (
  id int comment 'ID,唯一标识', # id是一行数据的唯一标识(不能重复) 
  username varchar(20) comment '用户名',
  name varchar(10) comment '姓名',
  age int comment '年龄',
  gender char(1) comment '性别'
  ) comment '用户表';
  ```

  

  #### 改

  添加一个字段

  ```sql
  alter table 表名 add 字段名 类型(长度) [comment '注释'] [约束];
  ```

  如:

  ```sql
  alter table tb_emp add qq varchar(11) comment 'QQ号码';
  ```

  修改数据类型

  ```sql
  alter table 表名 modify 字段名 新数据类型(长度) [comment '注释'] [约束];
  ```

  ```sql
  alter table 表名 change 旧字段名 新字段名 新数据类型(长度) [comment '注释'] [约束];
  ```

  如：

  修改QQ字段的字段类型，将其长度由11修改为13

  ```sql
  alter table tb_emp modify qq varchar(13) comment 'qq号码';
  ```

  修改qq字段名为 qq_num，字段类型varchar(13)

  ```sql
  alter table tb_emp change qq qq_num varchar(13) comment 'qq号码';
  ```

  

  **修改表名**

  ```sql
  rename table 表名 to 新表名;
  ```

  如:将当前的tb_emp 表的表名修改为emp

  ```sql
  rename table tb_emp to emp;
  ```

  #### 删

  ```sql
  alter table 表名 drop 字段名；
  ```

  如：删除tb_emp表中的qq_num字段

  ```sql
  alter table tb_emp drop qq_num;
  ```

  删除表

  ```sql
  drop table [if exists] 表名;
  ```

  

## 2、DML 数据库操作语言

#### 增

向指定字段添加数据

```sql
insert into 表名 （字段名1,字段名2） values （值1,值2);
```

全部字段添加数据

```sql
insert into 表名 values (值1,值2,...);
```

批量添加数据(指定字段)

```sql
insert into 表名 (字段名1,字段名2) values (值1,值2),(值1,值2);
```

批量添加数据(全部字段)

```sql
insert into 表名 values (值1, 值2, ...), (值1, 值2, ...);
```

#### 改

```sql
update 表名 set 字段名1=值1,字段名2=值2,... where条件;
```

#### 删

```sql
delete from 表名 where条件;
```



## 3、DQL 数据库查询语言

#### 全部语法

```
select 字段列表 from 表名列表 where 条件列表 group by 分组字段列表 having 分组后条件列表 order by 排序字段列表 limit 分页参数;
```

- 取别名 表名后写as 或者 空格后跟别名

  ```sql
  select t.* from test t; #给test表取了一个别名叫t
  ```

- 条件查询：查询 没有分配职位 的员工信息 *的性能没有把全部字段罗列出来高

  注意⚠️下方job 不能写为 job = null

  ```sql
  select * from tb_emp where job is null;
  ```


- between and 等于 >= <=

  ```sql
   select * from tb_emp where entrydate between '2000-01-01' and '2010-01-01';
  ```

- 多个or连接多个条件 可以用括号 （）代替

  ```sql
  select * from tb_emp where job=2 or job=3 or job=4;
  
  等于下面用括号
  select * from tb_emp where job in (2,3,4);
  ```

- 通配符_ 表示一个字符 通配符%表示多个字符

  ```sql
  select * from tb_emp where name like '_';
  
  查询姓张的人
  select * from tb_emp where name like '张%';
  ```

#### 聚合函数

- count 统计数量

  ```sql
  select count(*) from tb_emp;
  ```

- min 最小值

  统计企业最早入职的员工的日期

  ```sql
  select min(entrydate) from tb_emp;
  ```

- max 最大值

  统计企业最晚入职的员工的日期

  ```sql
  select max(entrydate) from tb_emp;
  ```

- avg 平均值

  统计该企业员工 ID 的平均值

  ```sql
  select avg(id) from tb_emp;
  ```

- sum 求和

  统计该企业员工的 ID 之和

  ```sql
  select sum(id) from tb_emp;
  ```

#### 分组查询

语法:

```sql
select 字段列表 from 表名 [where条件] group by 分组字段名 [having 分组后过滤条件];
```

如：根据性别分组 , 统计男性和女性员工的数量

```sql
select gender,count(*) from tb_emp group by gender;
```

查询入职时间在 '2015-01-01' (包含) 以前的员工 , 并对结果根据职位分组 , 获取员 工数量大于等于2的职位

```sql
select job,count(*) from tb_emp where entrydate <='2015-01-01' group by job having count(*)>=2;
```

#### 排序查询

语法:

```sql
select 字段列表 from 表名 where 条件列表 group by 分组字段 order by 字段1 排序方式1,字段2 排序方式2 ... ; 
```

ASC :升序(默认值)

DESC:降序



#### 分页查询

语法：

```sql
select 字段列表 from 表名 limit 起始索引,查询记录数;
```

起始索引从0开始。 计算公式 : 起始索引 = (查询页码 - 1)* 每页显示记录数



#### 多表查询

- 内连接

  隐式内连接:

  ```sql
  select 字段列表 from 表1,表2 where 表1.xx = 表2.xx;
  ```

  显式内连接：

  ```sql
  select 字段列表 from 表1 inner join 表2 on 表1.xx = 表2.xx;
  ```

- 外连接

  左连接

  ```sql
  select 字段列表 from 表1 left  join 表2 on 连接条件 ... ;
  ```

  右连接

  ```sql
  select 字段列表 from 表1 right join 表2 on 连接条件 ... ;
  ```

#### 子查询

sql语句中嵌套select语句 俗称套娃

```sql
SELECT * FROM t1 WHERE column1 = ( SELECT column1 FROM t2 ... );
```

1. 标量子查询(子查询结果为单个值[一行一列])
2. 列子查询(子查询结果为一列，但可以是多行)
3. 行子查询(子查询结果为一行，但可以是多列)
4. 表子查询(子查询结果为多行多列[相当于子查询结果是一张表])



- 标量子查询

  ```sql
  -- 1.查询"教研部"部门ID
   select id from tb_dept where name = '教研部'; #查询结果:2 
  
  -- 2.根据"教研部"部门ID, 查询员工信息
   select * from tb_emp where dept_id = 2;
  
  -- 合并出上两条SQL语句
   select * from tb_emp where dept_id = (select id from tb_dept where name = '教研部');
  ```

  

- 列子查询

  ```sql
  -- 1.查询"销售部"和"市场部"的部门ID
  select id from tb_dept where name = '教研部' or name = '咨询部'; #查询结果:3,2
  -- 2.根据部门ID, 查询员工信息
  select * from tb_emp where dept_id in (3,2);
  -- 合并以上两条SQL语句
  select * from tb_emp where dept_id in (select id from tb_dept where
  name = '教研部' or name = '咨询部');
  ```



- 行子查询

  ```sql
  -- 查询"韦一笑"的入职日期 及 职位
  select entrydate , job from tb_emp where name = '韦一笑'; #查询结果:2007-01-01 , 2
  -- 查询与"韦一笑"的入职日期及职位相同的员工信息
  select * from tb_emp where (entrydate,job) = ('2007-01-01',2);
  -- 合并以上两条SQL语句
  select * from tb_emp where (entrydate,job) = (select entrydate , job
  from tb_emp where name = '韦一笑');
  ```

- 表子查询

  ```sql
  -- 查询入职日期是 "2006-01-01" 之后的员工信息 , 及其部门信息
  
  -- 查询入职日期是 "2006-01-01" 之后的员工信息
  select * from emp where entrydate > '2006-01-01';
  
  --  合并以上两条SQL语句 基于查询到的员工信息，在查询对应的部门信息
  select e.*, d.* from (select * from emp where entrydate > '2006-01-
      01') e left join dept d on e.dept_id = d.id ;
  ```

#### 事务

特性:原子性，一致性，隔离性，持久性

- 原子性(Atomicity):事务是不可分割的最小单元，要么全部成功，要么全部失败。 
- 一致性(Consistency):事务完成时，必须使所有的数据都保持一致状态。
- 隔离性(Isolation):数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行。 
- 持久性(Durability):事务一旦提交或回滚，它对数据库中的数据的改变就是永久的。

使用事务控制删除部门和删除该部门下的员工的操作:

```sql
-- 开启事务
start transaction ;
-- 删除学工部
delete from tb_dept where id = 1;
-- 删除学工部的员工
delete from tb_emp where dept_id = 1;
```

上述的这组SQL语句，如果如果执行成功，则提交事务:

```sql
-- 提交事务 (成功时执行) 
commit ;
```

上述的这组SQL语句，如果如果执行失败，则回滚事务

```sql
rollback ;
```

#### 索引

##### 增

```sql
 create [ unique ] index 索引名 on 表名 (字段名,... ); #在添加索引时，也需要消耗时间
```

##### 查

```sql
show index from 表名;
```

##### 删

```sql
drop index 索引名 on 表名;
```

