---
title: mysql
date: 2023-07-28 21:21:26
categories: 数据库
tags: 
  - mysql
  - 数据库
cover: https://s2.loli.net/2024/06/07/HiWVoEZNrKnb4va.webp
---

# MySQL

## 执行编写顺序

```sql
-- 语句编写和执行顺序
select    -- 4
    字段列表
from    -- 1
    表名列表
where    -- 2
    条件列表
group by --3
    分组字段列表
having     
    分组后条件列表
order by --5
    排序字段列表
limit     --6
    分页参数
```

# 一、数据定义语言DDL

## 1、数据库操作

```sql
# DDL-数据库操作
show databases;                          #显示所有数据库
create database [if not exists] 数据库名;    #创建数据库
use 数据库名 ;                              #使用数据库
select database();                          #显示当前在哪个数据库下
drop database [if exists] 数据库名;            #删库跑路
```

## 2、表操作

```sql
-- DDL-表操作
show tables;# 显示所有表
create table 表名(字段 字段类型，字段 字段类型……) # 创建表
desc 表名 #查看表中所有字段
alter table 表名 add(添加字段)/modify(修改字段类型)/change(修改字段名及类型)/drop(删除字段)/rename to(修改表名);
drop table 表名;# 删除表
```

# 二、 DML（增删改）

## insert update delete

```sql
# 插入一条语句
insert into user(id, name, age, gender)
values (123, 'cxk', 25, '男');

insert into user(id, name, age, gender)
values (456, 'ikun', 21, '男');

# 插入多条语句
insert into user
values (456, 'ikun', 21, '男'),
(456, 'ikun', 21, '男'),
(456, 'ikun', 21, '男'),
(456, 'ikun', 21, '男');

# 查看表
select *from user;

# 修改 把name为ikun的name全部改为小黑子
update user
set name = '小黑子'
where name = 'ikun';

# 把name为cxk的 name改为鸡哥，性别改为女
update user
set name  = '鸡哥',
gender='女'
where name = 'cxk';

# 把所有人的年龄改为69
update user
set age=69;

# 删除 name为小黑子的人
delete from user where name = '小黑子';

# 删除所有人
delete from user;
```

# 三、DQL（查）

```sql
-- 创建表并插入数据--------------------------------------------

drop table if exists employee;
create table emp
(
id          int comment '编号',
workno      varchar(10) comment '工号',
name        varchar(10) comment '姓名',
gender      char(1) comment '性别',
age         tinyint unsigned comment '年龄',
idcard      char(18) comment '身份证号',
workaddress varchar(50) comment '工作地址',
entrydate   date comment '入职时间'
) comment '员工表';

-- 插入数据--------------------------------------------------------
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (1, '00001', '柳岩', '女', 20, '123456789012345678', '北京', '2000-01-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (2, '00002', '张无忌', '男', 18, '123456789012345670', '北京', '2005-09-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (3, '00003', '韦一笑', '男', 38, '123456789712345670', '上海', '2005-08-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (4, '00004', '赵敏', '女', 18, '123456757123845670', '北京', '2009-12-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (5, '00005', '小昭', '女', 16, '123456769012345678', '上海', '2007-07-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (6, '00006', '杨逍', '男', 28, '12345678931234567X', '北京', '2006-01-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (7, '00007', '范瑶', '男', 40, '123456789212345670', '北京', '2005-05-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (8, '00008', '黛绮丝', '女', 38, '123456157123645670', '天津', '2015-05-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (9, '00009', '范凉凉', '女', 45, '123156789012345678', '北京', '2010-04-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (10, '00010', '陈友谅', '男', 53, '123456789012345670', '上海', '2011-01-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (11, '00011', '张士诚', '男', 55, '123567897123465670', '江苏', '2015-05-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (12, '00012', '常遇春', '男', 32, '123446757152345670', '北京', '2004-02-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (13, '00013', '张三丰', '男', 88, '123656789012345678', '江苏', '2020-11-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (14, '00014', '灭绝', '女', 65, '123456719012345670', '西安', '2019-05-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (15, '00015', '胡青牛', '男', 70, '12345674971234567X', '西安', '2018-04-01');
INSERT INTO emp (id, workno, name, gender, age, idcard, workaddress, entrydate)
VALUES (16, '00016', '周芷若', '女', 18, null, '北京', '2012-06-01');
```

## 1、select from

```sql
-- 基本查询-------------------------------------------------------

-- 1、查询指定字段 name,workno,age返回
select name, workno, age
from emp;

-- 2、查询返回所有字段
select id,
 workno,
 name,
 gender,
 age,
 idcard,
 workaddress,
 entrydate
from emp;

-- 也可以查询所有字段，但实际使用中不建议，因为不明确
select *
from emp;

-- 3、查询所有员工的工作地址并 起一个别名(as可以省略)
select workaddress as '工作地址' from emp;

-- 4、查询所有员工的工作地址并去除重复地址
select distinct workaddress '上班地址' from emp;
```

## 2、where

```sql
-- 条件查询 where-------------------------------------------------

-- 1、查询年龄为88的员工
select * from emp where age=88;

-- 2、查询年龄小于20的员工
select *from emp where age<20;

-- 3、查询没有身份证号码的员工
select * from emp where idcard is null;

-- 4、查询有身份证号码的员工
select * from emp where idcard is not null;

-- 5、查询年龄不等于88的员工
select * from emp where age != 88;
select * from emp where age <> 88;

-- 6、查询年龄在15到20（两端都包含）之间的员工
select * from emp where age >= 15 && age <= 20;
select * from emp where age >= 15 and age <= 20;
-- between and 小的在前面 大的在后面 否则什么都查不到
select * from emp where age between 15 and 20;

-- 7、查询性别为女且年龄小于25的员工
select * from emp where gender = '女' and age < 25;

-- 8、查询年龄或等于18或20或40的员工
select * from emp where age = 18 ||age = 20 || age = 40;
select * from emp where age = 18 or age = 20 or age = 40;
select * from emp where age in (18,20,40);

-- 9、查询名字为两个字的员工的信息 模糊匹配：like _对应单个字符 %对应多个字符
select * from emp where name like '__';

-- 10、查询身份证号最后一位是X的员工
select * from emp where idcard like '%X';
# select * from emp where idcard like '_________________X'; 这样不太好

-- 11、查询身份证号倒数第九位是0的员工
select * from emp where idcard like '%0________';
```

## 3、count、max、group

```sql
-- 聚合函数 max min avg:最大最小平均值 sum:整列数据加起来 count:统计整列数据的个数 作用于列 所有聚合函数不参与null的计算--


-- 1、统计企业员工数量
select count(*) from emp; -- 16

select count(idcard) from emp; -- 15因为有一个员工的身份证号码为null

-- 2、统计所有员工的平均年龄
select avg(age) from emp;

-- 3、查询最大的年龄
select max(age) from emp;

-- 4、查询最小的年龄
select min(age) from emp;

-- 5、统计西安地区员工年龄之和
select sum(age) from emp where workaddress = '西安';

-- 分组查询 group by   [having 条件]

-- 1、根据性别分组，分别统计男女人数
-- 根据什么字段分组就查询什么字段，查询其他字段无意义
-- count(*)此处表示每组的数量
select gender,count(*) from emp group by gender;

-- 2、根据性别分组，分别统计男女平均年龄
select gender,avg(age) from emp group by gender;

-- 3、查询年龄小于45的员工，并根据工作地址分组，获取员工数量大于等于3的工作地址
-- having后面也接条件，与where不同的是，having对分组后的数据进行操作,having可以对聚合函数进行判断，where不行
-- 执行顺序：where>聚合函数>having 分组后查询字段一般为聚合函数或分组字段，因为查询其他字段无意义
select count(*) as address_count,workaddress from emp 
where age < 45 
group by workaddress having address_count >= 3;
```

## 4、order by

```sql
-- 排序查询 order by----------------------------------------------

-- 1、根据年龄对员工进行排序
-- 升序排序
select * from emp order by age; -- 默认升序排序
# select * from emp order by age asc;

-- 降序排序
select * from emp order by age desc;

-- 2、根据入职时间，对员工进行降序排序
select * from emp order by entrydate desc;

-- 3、根据年龄升序排序，若年龄相同，根据入职时间降序排序
select * from emp order by age, entrydate desc;

select * from emp order by name desc;

-- 分页查询(把所有数据一页一页展示出来) limit 起始索引，查询的个数--------->起始索引=（页码数-1）*每页展示数--------------------------------------------------------------------
-- 分页查询limit是MySQL的方言，不同的数据库语言有不同的实现

-- 1、查询第1页员工数据，查询个数为10
# select * from emp limit 0,10;
select *
from emp
limit 10;
-- 第一页的其实索引可以省略

-- 2、查询第2页员工数据，查询个数为10,不足10条则全部显示
select *
from emp
limit 10,10;
```

## 5、案例

```sql
-- DQL案例--------------------------------------------------------

-- 1.查询年龄为20,21,22,23岁的员工信息。
select *
from emp
where age = 20
or age = 21
or age = 22
or age = 23;
select *
from emp
where age in (20, 21, 22, 23);

-- 2.查询性别为男，并且年龄在20-40岁（含）以内的姓名为三个字的员工。
select *
from emp
where gender = '男'
and age between 20 and 40
and name like '___';

-- 3.统计员工表中，年龄小于60岁的，男性员工和女性员工的人数。
select gender, count(*)
from emp
where age < 60
group by gender;

-- 4.查询所有年龄小于等于35岁员工的姓名和年龄，并对查询结果按年龄升序排序，如果年龄相同按入职时间降序排序。
select name, age, entrydate
from emp
where age <= 35
order by age, entrydate desc;

-- 5.查询性别为男，且年龄在20-40岁（含）以内的员工信息，对查询的结果按年龄升序排序，年龄相同按入职时间升序排序,取前5个员工信息
select *
from emp
where gender = '男'
and age between 20 and 40;
```

# 四、 DCL

```sql
-- DCL---------------------------------------------------------

-- 创建用户
-- 创建用户‘tanking’，只能在当前主机localhost访问，密码是123456
create user 'tanking'@'localhost' identified by '123456';

-- 创建用户‘cxk’，能在任意主机访问数据库，密码 ctrljntm,%为通配符
create user 'cxk'@'%' identified by 'ctrljntm';

-- 修改cxk密码为‘123’
alter user 'cxk'@'%' identified with mysql_native_password by '123';

-- 删除用户‘tanking’
drop user 'tanking'@'localhost';

-- 查询权限---------------------------------------------------
show grants for cxk;

-- 给 用户cxk 授予 对clb数据库中所有表操作 的 所有权限
grant all on clb.* to 'cxk'@'%';

-- 撤销 cxk对clb数据库中所有表操作的所有权限
revoke all on clb.* from 'cxk'@'%';
```

# 五、 函数

## 1、字符串函数

```sql
-- 字符串函数----------------------------------------------
-- concat 拼接字符串
select concat('hello','mysql');

-- lower 字符串全部小写
select lower('HelloWorld');

-- upper 字符串全部大写
select upper('helloworld');

-- lpad 左填充 中间的参数表示 字符串的长度，最后参数表示 以什么字符填充
select lpad('cxk', 6, '-');

-- rpad 右填充 参数同上
select rpad('cxk', 6, '<');

-- trim 去除字符串头尾的空格
select trim('  cxk ctrl ');

-- substring(str,1,5) 截取字符串 此处表示截取 从索引1开始截取 字符串str 长度为5 的 子串（注意这个索引是从1开始的）
select substring('hello_mysql', 1, 5);
```

## 2、数值函数

```sql
-- 数值函数----------------------------------------------

-- ceil 向上取整，只要小数位不是0，就加1
select ceil(1.1);
select ceil(1.0);

-- floor 向下取整
select floor(1.9);

-- mod 模 （取余数）
select mod(6, 4);

-- rand 生成 0~1之间 的随机数
select rand();

-- round 四舍五入 保留位数
select round(2.305, 2);
-- 对2.305四舍五入，保留2位小数

-- 案例：生成一个6位数的随机验证码
select lpad(round(rand(), 6) * 1000000, 6, '0');
```

## 3、日期函数

```sql
-- 日期函数

-- curdate 返回当前日期
select curdate();

-- curtime 返回当前时间
select curtime();

-- now 返回当前时间和日期
select now();

-- year month day 分别获取 年月日
select year(now());
select month(now());
select day(now());

-- date_add 指定日期加上一定时间后的日期及时间
select date_add(now(), interval 66 day);    -- 从现在往后推66天

select date_add(now(), interval -10 year);  -- 从现在往前推10年

-- datediff 返回两个日期之间相差的天数(前面的日期-后面的日期)
select datediff(now(), '2002-10-26');   -- 从我生日到现在多少天了


-- 案例：查询所有员工入职天数，并按照降序排序
select name,datediff(curdate(),entrydate) as 入职天数 from emp order by 入职天数 desc;
```

## 4、流程控制函数

```sql
-- if(bool,val1,val2) true返回val1 false返回val2
select if(true, 'cxk', 'ikun');
-- 类似于三目运算符

-- ifnull(v1,v2) 如果v1不为null，返回v1,为null返回v2
select ifnull('ok', 'v2');

select ifnull('', 'v2'); -- 注意空字符串不是null

select ifnull(null, 'v2');

-- case when then else end
-- 查询员工姓名和工作地址，条件 北京/上海 ---> 一线城市 其他 ---> 二线城市
select name,
 (case workaddress
      when '北京' then concat(emp.workaddress, ' 一线城市')
      when '上海' then concat(emp.workaddress, ' 一线城市')
      else concat(emp.workaddress, ' 二线城市') end) as '工作地址'
from emp;
```

### 案例

```sql
-- 案例：统计班级各个学生的成绩，展示规则如下
-- >= 85 优秀
-- >= 60 及格
-- 否则 不及格

create table score
(
    id      int comment '编号',
    name    varchar(10) comment '姓名',
    math    tinyint unsigned comment '数学',
    English tinyint unsigned comment '英语',
    Chinese tinyint unsigned comment '语文'
) comment '成绩表';

insert into score
values (1, 'Tom', 67, 88, 95),
 (2, 'Rose', 23, 66, 90),
 (3, 'Jack', 56, 98, 76);

select name,
 (case when math >= 85 then '优秀' when math >= 60 then '及格' else '不及格' end) as '数学',
 (case when English >= 85 then '优秀' when English >= 60 then '及格' else '不及格' end) as '英语',
 (case when Chinese >= 85 then '优秀' when Chinese >= 60 then '及格' else '不及格' end) as '语文'
from score;
```

# 六、约束

## 1、基础约束

```sql
create table user2
(
id     int primary key auto_increment comment '主键',       
    -- primary key:主键，auto_increment:id随插入的数据自动增加，无需插入

 name   varchar(10) not null unique comment '姓名',          
    -- not null:不能为空，unique:唯一的，其他行不能出现重复

 age    int check ( age > 0 and age <= 120 ) comment '年龄', 
    -- check(条件):必须满足check中的条件

 status char(1) default '1' comment '状态',                  
    -- default:没有指定时的默认值

 gender char(1) comment '性别'
) comment '用户表';
```

```sql
-- 插入数据
insert into user2(name, age, status, gender) -- id为主键，无需插入，插入也没用，会自动更正
values ('Tom1', 10, 0, '男'),
('Tom2', 10, 0, '男');

insert into user2(name, age, status, gender)
values (null, 10, 0, '男'); -- 插入的name不能为null，报错

insert into user2 (name, age, status, gender)
values ('Tom1', 11, 0, '男'); -- 插入的name不能重复，报错,虽然没有成功，但是已经申请到主键，所以会空出一个主键

insert into user2(name, age, status, gender)
values ('Tom3', 80, 0, '男');

insert into user2(name, age, status, gender)
values ('Tom3', -1, 0, '男'); -- 插入的年龄超出0~120的范围，报错

insert into user2(name, age, gender)
values ('Tom4', 10, '男'); -- 不插入status，插入数据后status值默认为1
```

## 2、外键约束

### 1、增、删外键

```sql
-- 外键约束

-- 准备数据:准备两张表并插入数据

create table dept
(
id   int primary key auto_increment comment 'ID',
name varchar(50) not null comment '部门名称'
) comment '部门表';

insert into dept(id, name)
values (1, '研发部'),
 (2, '市场部'),
 (3, '财务部'),
 (4, '销售部'),
 (5, '总经办');

create table emp
(
id        int primary key auto_increment comment '主键',
name      varchar(10) not null unique comment '姓名',
age       int check ( age > 0 and age <= 120 ) comment '年龄',
job       varchar(10) comment '工作',
salary    int check ( salary > 0 ) comment '工资',
entrydate date comment '入职时间',
managerid int comment '直属领导ID',
dept_id   int comment '部门ID'
) comment '员工表';

insert into emp(id, name, age, job, salary, entrydate, managerid, dept_id)
values (1, '金庸', 66, '总裁', 20000, '2000-01-01', null, 5),
 (2, '张无忌', 20, '项目经理', 12500, '2005-12-05', 1, 1),
 (3, '杨逍 ', 33, '开发', 8400, '2000-11-03', 2, 1),
 (4, '韦一笑', 48, '开发 ', 11000, '2002-02-05', 2, 1),
 (5, '常遇春', 43, '开发', 10500, '2004-09-07', 3, 1),
 (6, '小昭', 19, '程序员鼓励师', 6600, '2004-10-12', 2, 1);

-- 添加外键
alter table emp add constraint fk_emp_dept_id foreign key (dept_id) references
dept(id);

-- 给emp表的dept_id添加外键约束，关联dept表的主键id，dept变为emp的主表
-- fk_emp_dept_id:外键名

-- 删除外键
alter table emp drop foreign key fk_emp_dept_id;
```

### 2、删除、更新行为

```sql
-- 删除、更新行为
-- 添加外键 + on cascade
alter table emp
add constraint fk_emp_dept_id foreign key (dept_id) references dept (id) on update cascade on delete cascade;

-- cascade 在父表中删除/更新外键记录（外键数据）时，检查是否有外键，若有，则子表中的数据也相应的删除/更新
-- 通俗：父表删除或更新外键，子表只要有与父表相关的外键，那么子表也跟着更新或删除
-- 删除研发部的外键id后，emp表中所有dept_id对应研发部的员工都会被删除

-- 添加外键 + on set null
alter table emp
add constraint fk_emp_dept_id foreign key (dept_id) references dept (id) on update set null on delete set null;

-- 通俗：父表删除或更新外键，子表只要有与父表相关的外键，那么子表直接设置为null
```

# 七、多表查询

## 1、概述

```sql
-- 创建dept表，并插入数据
create table dept
(
id   int auto_increment comment 'ID' primary key,
name varchar(50) not null comment '部门名称'
) comment '部门表';
INSERT INTO dept (id, name)
VALUES (1, '研发部'),
 (2, '市场部'),
 (3, '财务部'),
 (4,
  '销售部'),
 (5, '总经办'),
 (6, '人事部');
-- 创建emp表，并插入数据
create table emp
(
    id        int auto_increment comment 'ID' primary key,
    name      varchar(50) not null comment '姓名',
    age       int comment '年龄',
    job       varchar(20) comment '职位',
    salary    int comment '薪资',
    entrydate date comment '入职时间',
    managerid int comment '直属领导ID',
    dept_id   int comment '部门ID'
) comment '员工表';
-- 添加外键
alter table emp
add constraint fk_emp_dept_id foreign key (dept_id) references
  dept (id);
INSERT INTO emp (id, name, age, job, salary, entrydate, managerid, dept_id)
VALUES (1, '金庸', 66, '总裁', 20000, '2000-01-01', null, 5),
 (2, '张无忌', 20, '项目经理', 12500, '2005-12-05', 1, 1),
 (3, '杨逍', 33, '开发', 8400, '2000-11-03', 2, 1),
 (4, '韦一笑', 48, '开发', 11000, '2002-02-05', 2, 1),
 (5, '常遇春', 43, '开发', 10500, '2004-09-07', 3, 1),
 (6, '小昭', 19, '程序员鼓励师', 6600, '2004-10-12', 2, 1),
 (7, '灭绝', 60, '财务总监', 8500, '2002-09-12', 1, 3),
 (8, '周芷若', 19, '会计', 48000, '2006-06-02', 7, 3),
 (9, '丁敏君', 23, '出纳', 5250, '2009-05-13', 7, 3),
 (10, '赵敏', 20, '市场部总监', 12500, '2004-10-12', 1, 2),
 (11, '鹿杖客', 56, '职员', 3750, '2006-10-03', 10, 2),
 (12, '鹤笔翁', 19, '职员', 3750, '2007-05-09', 10, 2),
 (13, '方东白', 19, '职员', 5500, '2009-02-12', 10, 2),
 (14, '张三丰', 88, '销售总监', 14000, '2004-10-12', 1, 4),
 (15, '俞莲舟', 38, '销售', 4600, '2004-10-12', 14, 4),
 (16, '宋远桥', 40, '销售', 4600, '2004-10-12', 14, 4),
 (17, '陈友谅', 42, null, 2000, '2011-10-12', 1, null);

-- 多表查询 -- 笛卡尔积（A集合与B集合所有的组合的情况）

select *
from emp,
dept
where dept_id = dept.id;    -- 通过条件消除多余的笛卡尔积
```

## 2、连接查询

### 内连接

> 查询两张表中符合条件的数据，不符合条件的不会返回结果

```sql
-- 内连接演示
-- 查询员工姓名，并查询关联部门的名称（隐式内连接实现）
-- 表结构：emp，dept
-- 条件：emp.dept_id = dept.id

# select emp.name, dept.name
# from emp,
#      dept
# where dept_id = dept.id;
-- 给表起别名，起了别名后不能用原名
select e.name, d.name
from emp e,
dept d
where e.dept_id = d.id;


-- 查询员工姓名，并查询关联部门的名称（显式内连接实现） -- inner join...on... inner可以省略，on后面接条件
-- 表结构：emp，dept
-- 条件：emp.dept_id = dept.id

select e.name, d.name
from emp e
   join dept d on e.dept_id = d.id;
```

### 外连接

> 左外连接：查询==左表所有数据==和==右表中符合条件的数据==
>
> 右外连接：查询左表所有数据和右表中符合条件的数据

```sql
-- 外连接演示
-- 表结构：emp,dept
-- 条件：emp.dept_id = dept.id

-- 左外连接：查询左表所有数据和右、左表相交的数据 left [outer] join...on... outer可以省略，on后面接条件
-- 查询所有员工信息和对应部门信息
select e.name, d.name
from emp e
   left join dept d on d.id = e.dept_id;

-- 查询右表所有数据和左、右表相交的数据
select d.*, e.*
from emp e
   right join dept d on d.id = e.dept_id;
-- 左外连接和右外连接可以互相转化，表的顺序换一下就行了（习惯上左外连接）
```

### 自连接

```sql
-- 自连接（自连接时要给表起别名用于区分哪张表）

-- 表结构：emp
-- 内自连接
-- 查询员工及其所属领导的名字
select e1.name, e2.name
from emp e1,
emp e2
where e1.managerid = e2.id;

-- 外自连接
-- 查询员工及其所属领导的名字，没有领导也要查询出来
select a.name '员工', b.name '领导'
from emp a
   left join emp b on a.managerid = b.id;
```

## union查询

```sql
-- 联合查询 union ，union all
-- 将薪资低于5000和年龄大于50的员工全部查询出来

-- union all 相当于把两张表直接加起来，所以有可能有重复的员工
select name
from emp
where salary < 5000
union all
select name
from emp
where age > 50;

-- union 对union all结果去重
select name
from emp
where salary < 5000
union
select name
from emp
where age > 50;

-- 查询的列数必须相同，字段类型也必须相同
```

## 3、子查询

### 标量子查询

```sql
-- 标量子查询:后一个select查询的结果为单个值
-- 常用操作符:> < <> >= = <=
-- 查询 销售部 所有员工信息
-- a.先查询销售部门id
select id
from dept
where name = '销售部';
-- b.根据销售部门id查出员工信息
select *
from emp
where dept_id = (select id from dept where name = '销售部');


-- 查询在 方东白 入职之后的员工信息
select *
from emp
where entrydate > (select entrydate from emp where name = '方东白');
```

### 列子查询

```sql
-- 列子查询
-- 1、查询 销售部 和 市场部 的所有员工信息

-- a.先查询 销售部 和 市场部 的id
select id
from dept
where name = '销售部'
or name = '市场部';

-- b.根据id查询员工信息
select *
from emp
where dept_id in (select id from dept where name = '销售部' or name = '市场部');

-- 2、查询工资比财务部所有员工工资都高的员工

-- a.查询财务部所有人的工资
select salary
from emp
where dept_id = (select id from dept where name = '财务部');
-- b.根据上述信息查询比财务部所有人工资都高的员工
select *
from emp
where salary > all (select salary from emp where dept_id = (select id from dept where name = '财务部'));
--  > all :大于后面查询到的所有数据

-- 3、查询比研发部至少一人工资高的员工 任意一人：研发部至少存在一个人的工资比我低

select *
from emp
where salary > any
(select salary from emp where dept_id = (select id from dept where name = '研发部'));

select *
from emp
where salary > some
(select salary from emp where dept_id = (select id from dept where name = '研发部'));

-- any和some等同
```

### 行子查询

```sql
-- 行子查询
-- 查询与 张无忌 薪资和直属领导相同的员工信息

-- a.查询张无忌的薪资和直属领导
select salary, managerid
from emp
where name = '张无忌'


-- b.查询与 张无忌 薪资和直属领导相同的员工信息
select *
from emp
where (salary, managerid) = (select salary, managerid from emp where name = '张无忌');
```

### 表子查询

```sql
-- 表子查询
-- 查询和 鹿杖客 宋远桥 职位和薪资相同的员工信息

-- a.查询鹿杖客 宋远桥 的职位和薪资
select job, salary
from emp
where name in ('鹿杖客', '宋远桥');

-- b.查询和 鹿杖客 宋远桥 职位和薪资相同的员工信息
select *
from emp
where (job, salary) in (select job, salary from emp where name in ('鹿杖客', '宋远桥'));

-- 查询入职时间是‘2006-01-01’之后的员工及其部门信息
-- a.查询入职时间是‘2006-01-01’之后的员工
select *
from emp
where entrydate > '2006-01-01';
-- b.查询这些员工的部门信息
-- 把上述查询的结果当做需要查询的一张表
select e.*, d.*
from (select * from emp where entrydate > '2006-01-01') e
   left join dept d on e.dept_id = d.id;
```

## 4、多表查询案例

```sql
-- --------------->多表查询案例 <--------------
-- 准备数据
create table salgrade
(
grade int,
losal int,
hisal int
) comment '薪资等级表';
insert into salgrade
values (1, 0, 3000);
insert into salgrade
values (2, 3001, 5000);
insert into salgrade
values (3, 5001, 8000);
insert into salgrade
values (4, 8001, 10000);
insert into salgrade
values (5, 10001, 15000);
insert into salgrade
values (6, 15001, 20000);
insert into salgrade
values (7, 20001, 25000);
insert into salgrade
values (8, 25001, 30000);


-- 1.查询员工的姓名、年龄、职位、部门信息。
select e.name, e.age, e.job, d.*
from emp e
   left join dept d on e.dept_id = d.id;

-- 2.查询年龄小于30岁的员工姓名、年龄、职位、部门信息。
select e.name, e.age, e.job, d.*
from emp e
   join dept d on e.dept_id = d.id
where e.age < 30;

-- 3.查询拥有员工的部门ID、部门名称。
select distinct d.id, d.name
from dept d,
emp e
where d.id = e.dept_id;

-- 4.查询所有年龄大于40岁的员工，及其归属的部门名称：如果员工没有分配部门，也需要展示出来。
select e.*, d.name
from emp e
   left join dept d on e.dept_id = d.id
where e.age > 40;

-- 5.查询所有员工的工资等级。
select e.name, e.salary, s.losal, s.hisal, s.grade
from emp e
   left join salgrade s on e.salary between s.losal and s.hisal;

-- 6.查询"研发部”所有员工的信息及工资等级。

select *
from emp e,
salgrade s,
dept d
where e.dept_id = d.id
and e.salary between s.losal and s.hisal
and d.name = '研发部';

select e.*, grade
from (select * from emp e where e.dept_id = (select id from dept d where d.name = '研发部')) e
   left join salgrade s on e.salary <= s.hisal and e.salary >= s.losal;

-- 7.查询"研发部”员工的平均工资。
select avg(e.salary)
from emp e,
dept d
where e.dept_id = d.id
and d.name = '研发部';

-- 8.查询工资比"灭绝"高的员工信息。
-- 子查询
select *
from emp
where salary > (select salary from emp where name = '灭绝');

-- 自查询
select *
from emp a,
emp b
where a.salary > b.salary
and b.name = '灭绝';

-- 9.查询比平均薪资高的员工信息。
select avg(salary)
from emp;

select *
from emp
where salary > (select avg(salary) from emp);

-- 10.查询低于本部门平均工资的员工信息。
-- 查询某部门平均薪资
select avg(e.salary)
from emp e,
dept d
where e.dept_id = d.id
and d.id = 1;
-- 查询低于本部门平均工资的员工信息。
select *
from emp e1
where e1.salary < (select avg(e2.salary)
             from emp e2,
                  dept d
             where e2.dept_id = d.id
               and d.id = e1.dept_id);

-- 11.查询所有的部门信息，并统计部门的员工人数。
-- 查询所有部门信息
select *
from dept;
-- 统计1号部门员工数量
select count(*)
from emp
where dept_id = 1;
-- 查询所有的部门信息，并统计部门的员工人数
select d.id, d.name, (select count(*) from emp e where e.dept_id = d.id) '人数'
from dept d;
```

# 八、事务

## 1、事务简介

> 事务 是一组操作的集合，它是一个不可分割的工作单位，事务会把所有的操作作为一个整体一起向系 统提交或撤销操作请求，即这些操作要么同时成功，要么同时失败。

## 2、事务操作

```sql
-- 事务

-- ---> 准备数据 <---------------------
create table account
(
id    int primary key AUTO_INCREMENT comment 'ID',
name  varchar(10) comment '姓名',
money double(10, 2) comment '余额'
) comment '账户表';

insert into account(name, money)
VALUES ('张三', 2000),
 ('李四', 2000);

-- -----------------------------------

-- 恢复数据操作
update account
set money = 2000
where name in ('张三', '李四');


select @@autocommit; -- 查询事务提交方式 0-手动 1-自动

-- 方式一
set @@autocommit = 0;-- 设置为手动提交


-- 转账操作（张三转给李四1000元）
-- 查询张三余额
select money
from account
where name = '张三';

-- 张三余额-1000
update account
set money = money - 1000
where name = '张三';

-- 若此处出现异常，则张三-1000元，但李四并没有+1000元
异常... -- 模拟出错场景

-- 李四余额+1000
update account
set money = money + 1000
where name = '李四';

-- 提交事务
commit;
-- 回滚事务
rollback;

-- 方式二

set @@autocommit = 1; -- 设置为自动提交

start transaction;

-- 转账操作（张三转给李四1000元）
-- 查询张三余额
select money
from account
where name = '张三';

-- 张三余额-1000
update account
set money = money - 1000
where name = '张三';

-- 若此处出现异常，则张三-1000元，但李四并没有+1000元
异常...   -- 模拟出错场景

-- 李四余额+1000
update account
set money = money + 1000
where name = '李四';

-- 提交事务
commit;
-- 回滚事务
rollback;
```

## 3、事务的四大特性（ACID）

> 1. **原子性（Atomicity）：事务是不可分割的最小操作单元，要么全部成功，要么全部失败。**

> 2. **一致性（Consistency）：事务完成时，必须使所有的数据都保持一致状态。**
>
> 2. **一致性（Consistency）：事务完成时，必须使所有的数据都保持一致状态。**

>3. **隔离性（Isolation）：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立 环境下运行。**
>
>4. **持久性（Durability）：事务一旦提交或回滚，它对数据库中的数据的改变就是永久的。**

## 4、并发事务问题

> **脏读：一个事务读取到另外一个事务未提交的数据。**
>
> **不可重复读：一个事务先后读取同一条数据，但两次读取到的数据不同。**
>
> **幻读：一个事务查询数据时没有对应数据，插入该数据时又发现该数据已经存在，好像出现“幻影”。**

## 5、隔离级别

```sql
-- 并发事务与隔离级别

-- 查询事务隔离级别
select @@transaction_isolation;

-- 设置事务隔离级别 session-仅当前页面起作用    transaction isolation level-事务隔离级别
set session transaction isolation level read uncommitted;

-- repeatable read是默认级别
set session transaction isolation level repeatable read ;

-- 隔离级别越高，数据越安全，但是性能越低
```

# 九、JDBC

## 1、jdbc7步编程

```java
//JDBC快速入门
//1、注册驱动
Class.forName("com.mysql.cj.jdbc.Driver");

//2、获取连接
String url = "jdbc:mysql://localhost:3306/clb";
String username = "root";
String password = "123456";
Connection conn = null;
conn = DriverManager.getConnection(url, username, password);


//3、定义sql
String sql = "update account set money = 666 where id = 1";

//4、获取执行sql的对象 Statement
Statement stmt = null;
stmt = conn.createStatement();


//5、执行sql
int count = 0;//受影响的行数
count = stmt.executeUpdate(sql);


//6、处理结果
System.out.println("受影响的行数有" + count + "行");

//7、释放资源
stmt.close();
conn.close();
```

## 2、DriverManager

```java
  //1、注册驱动 -->可以省略
  //Class.forName("com.mysql.cj.jdbc.Driver");

  //2、获取连接 -->如果连接的是本机的数据库且端口是默认的 3306 则可以简写
  // String url = "jdbc:mysql://localhost:3306/clb";
  //简写：
  String url = "jdbc:mysql:/clb";
  String username = "root";
  String password = "123456";
  Connection conn = DriverManager.getConnection(url, username, password);


  //3、定义sql
  String sql = "update account set money = 1000 where id = 1";

  //4、获取执行sql的对象 Statement
  Statement stmt = null;
  stmt = conn.createStatement();


  //5、执行sql
  int count = 0;//受影响的行数
  count = stmt.executeUpdate(sql);


  //6、处理结果
  System.out.println("受影响的行数有" + count + "行");

  //7、释放资源

  stmt.close();
  conn.close();
/*
    DriverManager有两个作用:
    1、注册驱动
    2、获取数据库连接
*/
```

## 3、Connection

```java
//1、管理事务       
try {
    //开启事务
    conn.setAutoCommit(false);//将自动提交改为手动

    //5、执行sql
    int count = stmt.executeUpdate(sql);
    //6、处理结果
    System.out.println("受影响的行数有" + count + "行");

    //制造异常
    int i = 3 / 0;

    //5、执行sql
    int count2 = stmt.executeUpdate(sql2);
    //6、处理结果
    System.out.println("受影响的行数有" + count2 + "行");

    //提交事务
    conn.commit();
} catch (Exception e) {
    //回滚事务
    conn.rollback();
    e.printStackTrace();
}
```

## 4、Statement

```java
package JDBC;

import org.junit.Test;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.Statement;

public class Demo3Statement {
/*
  Statement:
  1、执行SQL语句
  int executeUpdate(sql) -> 执行DML、DDL语句
  执行DML语句
*/

@Test
public void testDML() throws Exception {
  //1、注册驱动
  //Class.forName("com.mysql.cj.jdbc.Driver");

  //2、获取连接
  String url = "jdbc:mysql://localhost:3306/clb";
  String username = "root";
  String password = "123456";
  Connection conn = null;
  conn = DriverManager.getConnection(url, username, password);


  //3、定义sql
  String sql = "update account set money = 666 where id = 1";

  //4、获取执行sql的对象 Statement
  Statement stmt = conn.createStatement();

  //5、执行sql
  int count = stmt.executeUpdate(sql);

  //6、处理结果
  System.out.println(count > 0 ? "修改成功" : "修改失败");

  //7、释放资源
  stmt.close();
  conn.close();
}


/*
  执行DDL语句
*/

@Test
public void testDDL() throws Exception {
  //1、注册驱动
  //Class.forName("com.mysql.cj.jdbc.Driver");

  //2、获取连接
  String url = "jdbc:mysql://localhost:3306/clb";
  String username = "root";
  String password = "123456";
  Connection conn = null;
  conn = DriverManager.getConnection(url, username, password);

  //3、定义sql
  String sql = "drop database cxk";

  //4、获取执行sql的对象 Statement
  Statement stmt = conn.createStatement();

  //5、执行sql
  int count = stmt.executeUpdate(sql);

  //6、处理结果
//        System.out.println(count > 0 ? "修改成功" : "修改失败");
  System.out.println(count);

  //7、释放资源

  stmt.close();
  conn.close();
}
}
```

## 5、ResultSet

```java
package JDBC;

import org.junit.Test;
import pojo.Account;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;
import java.util.ArrayList;

public class Demo4ResultSet {
/*
  ResultSet:
  执行DQL查询语句
*/

@Test
public void ResultSetDemo() throws Exception {
  //1、注册驱动
  //Class.forName("com.mysql.cj.jdbc.Driver");

  //2、获取连接
  String url = "jdbc:mysql://localhost:3306/clb";
  String username = "root";
  String password = "123456";
  Connection conn = null;
  conn = DriverManager.getConnection(url, username, password);
  //3、定义sql语句
  String sql = "select * from account";
  //4、获取statement对象
  Statement stmt = conn.createStatement();
  //5、执行sql
  ResultSet rs = stmt.executeQuery(sql);

  //6、处理数据，遍历rs中的所有数据
  //6.1光标向下移动一行，判断当前行是否有数据
  /*
  while (rs.next()) {
      //6.2 获取数据getXxx()
      int id = rs.getInt(1);
      String name = rs.getString(2);
      double money = rs.getDouble(3);

      System.out.println(id);
      System.out.println(name);
      System.out.println(money);
      System.out.println("-------------------");
  }
   */
  while (rs.next()) {
      //6.2 获取数据getXxx()
      int id = rs.getInt("id");
      String name = rs.getString("name");
      double money = rs.getDouble("money");

      System.out.println(id);
      System.out.println(name);
      System.out.println(money);
      System.out.println("-------------------");
  }

  //7、释放资源
  rs.close();
  stmt.close();
  conn.close();
}

  /*
  查询数据库中account表的数据，封装成Account对象，并存储到ArrayList表中
  1、定义实体类Account
  2、查询数据
  3、封装数据到ArrayList表中
*/

@Test
public void ResultSetDemo2() throws Exception {
  //1、注册驱动
  //Class.forName("com.mysql.cj.jdbc.Driver");

  //2、获取连接
  String url = "jdbc:mysql://localhost:3306/clb";
  String username = "root";
  String password = "123456";
  Connection conn = null;
  conn = DriverManager.getConnection(url, username, password);
  //3、定义sql语句
  String sql = "select * from account";
  //4、获取statement对象
  Statement stmt = conn.createStatement();
  //5、执行sql
  ResultSet rs = stmt.executeQuery(sql);

  ArrayList<Account> lst = new ArrayList<>();

  while (rs.next()) {
      //6.2 获取数据getXxx()
      int id = rs.getInt("id");
      String name = rs.getString("name");
      double money = rs.getDouble("money");
      lst.add(new Account(id, name, money));
  }

  System.out.println(lst);

  //7、释放资源
  rs.close();
  stmt.close();
  conn.close();
}

}
```

## 6、用户登陆

```java
package JDBC;

import org.junit.Test;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

/*
用户登陆
*/

public class UserLoginDemo5 {

@Test
public void UserLogin() throws Exception {
  //1、注册驱动
  Class.forName("com.mysql.cj.jdbc.Driver");

  //2、获取连接
  String url = "jdbc:mysql://localhost:3306/clb";
  String username = "root";
  String password = "123456";
  Connection conn = null;
  conn = DriverManager.getConnection(url, username, password);

  //接收用户名和密码
  String name = "cxk";
  String pwd = "jntm";

  String sql = "select * from tb_user where name = '" + name + "' and password = '" + pwd + "'";

  //获取Statement对象
  Statement stmt = conn.createStatement();

  //执行sql语句
  ResultSet rs = stmt.executeQuery(sql);

  //判断是否登陆成功
  System.out.println(rs.next() ? "登陆成功" : "登陆失败");

  //7、释放资源
  rs.close();
  stmt.close();
  conn.close();
}

/*
  sql注入问题
*/

@Test
public void UserLogin_Inject() throws Exception {
  //1、注册驱动
  Class.forName("com.mysql.cj.jdbc.Driver");

  //2、获取连接
  String url = "jdbc:mysql://localhost:3306/clb";
  String username = "root";
  String password = "123456";
  Connection conn = null;
  conn = DriverManager.getConnection(url, username, password);

  //接收用户名和密码 sql注入->此处密码错误却还能成功登陆
  String name = "ikunngmngmngm";
  String pwd = "'or '1' = '1";

  String sql = "select * from tb_user where name = '" + name + "' and password = '" + pwd + "'";

  //获取Statement对象
  Statement stmt = conn.createStatement();

  //执行sql语句
  ResultSet rs = stmt.executeQuery(sql);
  System.out.println("sql = " + sql);

  //判断是否登陆成功
  System.out.println(rs.next() ? "登陆成功" : "登陆失败");

  //7、释放资源
  rs.close();
  stmt.close();
  conn.close();
}
```

## 7、PrepareStatement

```java
package JDBC;

import org.junit.Test;

import java.sql.*;

/*
    PrepareStatement解决sql注入问题
    将sql语句中的敏感符号加\ 进行转义
*/

public class PrepareStatementDemo6 {

    @Test
    public void PrepareStatement() throws Exception {
        //1、注册驱动
        Class.forName("com.mysql.cj.jdbc.Driver");

        //2、获取连接
        //useSeverPrepStmts=true 开启预编译功能 ：性能更高
        String url = "jdbc:mysql:/clb?useSSL=false&useSeverPrepStmts=true";
        String username = "root";
        String password = "123456";
        Connection conn = null;
        conn = DriverManager.getConnection(url, username, password);

        //接收用户名和密码 sql注入->此处密码错误却还能成功登陆
        String name = "ikunngmngmngm";
        String pwd = "'or '1' = '1";

        //定义sql语句
        String sql = "select * from tb_user where name = ? and password = ?";

        //获取Statement对象
        PreparedStatement pstmt = conn.prepareStatement(sql);

        //设置？的值
        pstmt.setString(1, name);
        pstmt.setString(2, pwd);

        //执行sql语句
        ResultSet rs = pstmt.executeQuery();

        //判断是否登陆成功
        System.out.println(rs.next() ? "登陆成功" : "登陆失败");

        //7、释放资源
        rs.close();
        pstmt.close();
        conn.close();
    }
}
```

## 8、Druid数据库连接池

```java
package Druid;

/*
 * Druid数据库连接池
 * */

import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.FileInputStream;
import java.sql.Connection;
import java.util.Properties;

public class DruidDemo {
    public static void main(String[] args) throws Exception {
        //1、导入jar包
        //2、定义配置文件
        //3、加载配置文件

        //4、获取连接池对象
        Properties prop = new Properties();
        prop.load(new FileInputStream("jdbcDemo/src/druid.properties"));
        DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);

        //5、获取数据库连接
        Connection connection = dataSource.getConnection();
        System.out.println(connection);


//        System.out.println(System.getProperty("user.dir"));
    }
}

//配置文件（根据具体情况配置）
driverClassName=com.mysql.cj.jdbc.Driver
url=jdbc:mysql:/clb?useSSL=false&useServerPrepStmts=true
username=root
password=123456
# 初始化连接数量
initialSize=5
# 最大连接数量
maxActive=10
# 最大等待时间->3秒
maxWait=30000
```

# 十、进阶

##  1、索引

```mysql
-- 索引

-- 查询表中所有索引
show index from tb_user;

-- 创建索引:  create index 索引名 on 表名(字段名);
-- 给表中name字段创建常规索引
create index idx_user_name on tb_user (name);

-- 给表中phone字段创建唯一索引
create unique index idx_user_phone on tb_user (phone);

-- 给表中profession,age,status创建联合索引,字段的顺序是有讲究的
create index idx_user_pro_age_sta on tb_user (profession, age, status);

-- 给表中email字段创建常规索引
create index idx_user_email on tb_user (email);

-- 删除索引:  drop index 索引名 on 表名;
drop index idx_user_email on tb_user;


-- SQL性能分析
-- 1、查看各种sql语句的执行频率
show global status like 'Com_______';

-- 2、慢查询日志,查询语句执行时间超过指定时间（默认10秒）就会记录到慢查询日志
show variables like 'slow_query_log';



-- 3、profile 详情
-- 查看是否支持profiling
select @@have_profiling;

-- 查看profiling是否开启 0-未开启 1-开启
select @@profiling;

-- 开启profiling
set profiling = 1;

select *
from tb_user;


-- 查看所有查询语句耗时情况
show profiles;

-- 查看指定查询语句的各个阶段的耗时
-- show profile for query 10;

-- 额外查看sql语句cpu消耗
-- show profile cpu for query 10;


-- 4、explain 执行计划

explain
select *
from tb_user
where id = 1;


-- 索引的使用
/**
1-最左前缀法则：
 最左边的索引必须存在，否则索引全部失效
  如果跳过了某个索引，那么从该索引开始的后面字段的索引失效
*/

show index from tb_user;

-- 没有跳过索引，索引全部生效
explain
select *
from tb_user
where profession = '软件工程'
and age = 31
and status = '0';

explain
select *
from tb_user
where profession = '软件工程'
and age = 31;

explain
select *
from tb_user
where profession = '软件工程';

-- 跳过最左边的索引，索引全部失效
explain
select *
from tb_user
where age = 31
and status = '0';

-- 跳过age索引，后面的status索引失效，前面的profession索引正常
explain
select *
from tb_user
where profession = '软件工程'
and status = '0';

-- 查询语句的条件的位置不影响索引是否生效
-- 没有跳过任何索引，索引全部生效
explain
select *
from tb_user
where status = '0'
and age = 31
and profession = '软件工程';

/**
2-出现范围查询(>,<)时，后面的索引失效,使用>=,<=不会
*/

explain
select *
from tb_user
where profession = '软件工程'
and age > 30
and status = '0';

/**
3-索引运算
    ·不要在索引上进行运算，否则索引失效
    ·字符串字段查询时不加单引号，索引失效
*/

explain
select *
from tb_user
where substring(phone, 10, 2) = '15';

explain
select *
from tb_user
where phone = 17799990004;
-- 没用单引号，索引失效

/**
4-模糊匹配
    ·后面加%/_模糊匹配，索引正常
    ·前面模糊，索引失效
*/

explain
select *
from tb_user
where profession like '软件%'; -- 索引有效


explain
select *
from tb_user
where profession like '%工程';

/**
5-or连接的条件
    ·只有两个条件都有索引，索引才会生效，否则失效
    ·解决方法：建立相关字段的索引
*/
explain
select *
from tb_user
where phone = '17799990004'
or age = 31; -- phone有索引，但是age没有索引，索引失效

explain
select *
from tb_user
where phone = '17799990004'
or id = 1;
-- phone 和 id都有索引，索引生效


/**
6-数据分布影响
·如果MySQL评估使用索引比全表更慢，则放弃索引，大部分数据都符合条件时会出现这种情况
*/

explain
select *
from tb_user
where phone > '0';
-- 所有电话号码都大于0，使用全表扫描

-- 使用了索引，具体使用索引还是不使用索引取决表中的数据，符合条件的数据多->不用，少->用
explain
select *
from tb_user
where profession is null;

-- 未使用索引
explain
select *
from tb_user
where profession is not null;



-- SQL提示:是优化数据库的一个重要手段，就是在sql语句中加入一些人为的提示达到优化操作的目的
-- 查询一个字段时，如果同时存在单列索引和联合索引，默认使用联合索引

-- profession有两个索引，默认使用的是联合索引
explain
select *
from tb_user
where profession = '软件工程';

-- use index 建议MySQL使用这个索引，至于用不用，取决于MySQL
explain
select *
from tb_user use index (idx_user_pro)
where profession = '软件工程';

-- ignore index 忽略索引，指定不会使用该索引
explain
select *
from tb_user ignore index (idx_user_pro)
where profession = '软件工程';

-- force index 强制使用，指定必须使用该索引,use可能MySQL并不接受，force强制MySQL使用

-- ---------------------------------------------------------------

```

## 2、SQL优化

```mysql
-- SQL优化

-- 1、插入优化:
-- 批量插入
-- 手动控制事务
-- 主键顺序插入
-- load指令

select @@local_infile;
set global local_infile = 1;

load data local infile 'D:/Edge下载/进阶篇/相关SQL脚本/load_user_100w_sort.sql' into table tb_user fields terminated by ',' lines terminated by '\n';

-- 2、主键优化：
-- 尽量降低主键的长度
-- 插入数据时，尽量使用顺序插入
-- 尽量不要使用UUID或者其他自然主键
-- 尽量不对主键进行修改

-- 3、order by 优化：
-- using filesort:效率低
-- using index:效率高
show index from  tb_user; -- Collation:索引的顺序，A->升序，D->降序

explain select age,phone from tb_user order by age,phone;

-- 创建age和phone字段的一个联合索引,根据索引查询，结果不需要再进行排序
create index idx_user_age_phone on tb_user(age,phone);

-- 两个条件都是倒序，使用index倒序扫描 Backward index scan; Using index
explain select age,phone from tb_user order by age desc,phone desc ;

-- 先对phone排序，再根据age排序,不符合最左前缀法则，Using index; Using filesort
explain select age,phone from tb_user order by phone,age;

-- 先对age升序，再对phone降序排序
explain select age,phone from tb_user order by age,phone desc;

-- 优化：创建age升序phone降序的索引(默认是升序)
create index idx_user_age_phone_ad on tb_user(age,phone desc);

-- 尽量避免使用select*,否则需要所有字段都建立了联合索引才能走索引，否则就是filesort
-- 不可避免使用filesort时，可以适当增大排序文件缓冲区大小

-- limit 优化
select *
from tb_user
limit 900000,10;

-- 覆盖索引加子查询的形式
select t.*
from tb_user t,
(select id from tb_user order by id limit 900000,10) e
where t.id = e.id;


-- count()优化
-- 效率 count(*) ≈ count(1) > count(主键) > count(字段)
select count(*)from tb_user;
select count(id)from tb_user;
select count(1)from tb_user;
select count(username) from tb_user;

-- update 优化
-- update更新的数据最好有索引，否则执行update语句时行锁会升级为表锁，并发性能降低
```
