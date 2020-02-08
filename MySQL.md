### MySQL数据库


![](C:\Users\wang1\Desktop\mdSource\数据库\SQL分类.bmp)

#### 第一章 数据库

##### 1.1 概念

​	用于存储和管理数据的仓库

##### 1.2 特点

1. 持久化存储数据的。其实数据库就是一个文件系统
2. 方便存储和管理数据
3. 使用了统一的方式操作数据库  ———  SQL

##### 1.3 配置

1. MySQL服务

   ```cmd
   net start mysql：启动mysql服务
   net stop mysql：关闭mysql服务
   ```

2. MySQL登录

   ```cmd
   mysql -uroot -p 密码
   mysql -h ip -u root -p 连接目标的密码
   ```

3. MySQL退出

   ```cmd
   exit
   quit
   ```

#### 第二章 SQL

##### 2.1 概念

​	Structured Query Language：结构化查询语言

##### 2.2 语法

- SQL 语句可以单行或多行书写，以分号结尾
- 可使用空格和缩进来增强语句的可读性
- MySQL 数据库的 SQL 语句不区分大小写，关键字建议使用大写
- 注释
  - 单行注释: -- 注释内容    或   # 注释内容(mysql 特有) 
  - 多行注释:  /* 注释 */

##### 2.3 分类

- DDL (Data Definition Language) 数据定义语言

  ​		定义数据库对象：数据库，表，列等。			  关键字：create, drop,alter 等

- DML (Data Manipulation Language) 数据操作语言

  ​		对数据库中表的数据进行增删改。					  关键字：insert, delete, update 等

- DQL (Data Query Language) 数据查询语言

  ​		查询数据库中表的记录(数据)。						   关键字：select, where 等

- DCL (Data Control Language) 数据控制语言

  ​		数据库的访问权限和安全级别及创建用户  		关键字：GRANT， REVOKE 等



#### 第三章 DDL操作数据库、表

##### 3.1 操作数据库（CRUD）

- C (Create)：创建

  ```sql
  create database 数据库名称;
  create database if not exists 数据库名称;
  create database 数据库名称 character set 字符集名;
  ```

- R (Retrieve)：查询

  ```sql
  show databases;
  
  -- 查询某个数据库的字符集:查询某个数据库的创建语句
  show create database 数据库名称;
  ```

- U (Update)：修改

  ```sql
  alter database 数据库名称 character set 字符集名称;
  ```

- D (Delete)：删除

  ```sql
  drop database 数据库名称;
  drop database if exists 数据库名称;
  ```

- 使用数据库

  ```sql
  -- 查询当前正在使用的数据库名称
  select database();
  
  -- 使用数据库
  use 数据库名称;
  ```

##### 3.2 操作表

- C (Create)：创建表

  > 数据库数据类型：
  >
  > 1. int
  > 2. double(size, d)
  > 3. date ：yyyy-MM-dd
  > 4. datetime  ：日期  yyyy-MM-dd HH:mm:ss
  > 5. timestamp ： 时间戳类型  yyyy-MM-dd HH:mm:ss
  > 6. varchar 

  ```
  create table 表名(
      列名1 数据类型1,
      列名2 数据类型2,
      ....
      列名n 数据类型n
  );
  ```

  ```sql
  -- 复制表
  create table 表名 like 被复制的表名;
  ```

- R (Retrieve)：查询

  ```sql
  -- 查询某个数据库中所有的表名称
  show tables;
  -- 查询表结构
  desc 表名;
  ```

- U (Update)：修改

  ```sql
  -- 修改表名
  alter table 表名 rename to 新表名;
  -- 修改表的字符集
  alter table 表名 character set 字符集名称;
  -- 添加列
  alter table 表名 add 列名 数据类型;
  -- 删除列
  alter table 表名 drop 列名;
  -- 修改列的名称
  alter table 表名 change 列名 新列名 新数据类型;
  -- 修改列的类型
  alter table 表名 modify 列名 新数据类型;
  ```

- D (Delete)：删除

  ```sql
  drop table 表名;
  drop table if exists 表名;
  ```

  

#### 第四章 DML增删表中数据

##### 4.1 添加数据

```sql
insert into 表名(列名1,列名2,...列名n) values(值1,值2,...值n);
insert into 表名 values(值1,值2,...值n);

注意:
1、列名和值要一一对应,表名后定义列名则默认给所有列添加值
2、除了数字类型，其他类型需要使用引号(单双都可以)引起来
```

##### 4.2 删除数据

```sql
delete from 表名 [where 条件]

注意:
1、如果不加条件，则删除表中所有记录
2、删除所有记录
	delete from 表名;	   -- 不推荐使用，有多少条记录就会执行多少次删除操作
	TRUNCATE TABLE 表名; -- 推荐使用，效率更高 先删除表，然后再创建一张一样的表。
```

> DELETE、TRUNCATE和DROP区别
>
> DELETE语句执行删除的过程是每次从表中删除一行，并且同时将该行的删除操作作为事务记录在日志中保存以便进行进行回滚操作。
>
> TRUNCATE TABLE 则一次性地从表中删除所有的数据并不把单独的删除操作记录记入日志保存，删除行是不能恢复的。并且在删除的过程中不会激活与表有关的删除触发器。执行速度快。
>
> DROP语句删除表结构及所有数据，并将表所占用的空间全部释放。

##### 4.3 修改数据

```sql
update 表名 set 列名1 = 值1, 列名2 = 值2,... [where 条件];

注意: 
如果不加任何条件，则会将表中所有记录全部修改。
```



#### 第五章 DQL查询表中的记录

##### 5.1 语法

```
select 
	字段列表 
from 
	表名列表 
where 
	条件列表 
group by 
	分组字段 
having 
	分组之后的条件 
order by 
	排序 
limit 
	分页限定
```

##### 5.2 基础查询

```sql
-- 多个字段的查询
select 字段名1，字段名2 ... from 表名;
-- 去除重复
distinct
-- 计算列
一般可以使用四则运算计算一些列的值。（一般只会进行数值型的计算）
-- 起别名
as (可以省略)
```

##### 5.3 条件查询

```sql
> 、< 、<= 、>= 、= 、<>
BETWEEN...AND  
IN(集合) 
LIKE：模糊查询
占位符：
	_ : 单个任意字符
	% : 多个任意字符
IS NULL  
and 或 &&
or 或 || 
not 或 !
```

```sql
-- 查询年龄大于20岁
select * from student where age > 20;

-- 查询年龄等于20岁
select * from student where age = 20;

-- 查询年龄不等于20岁
select * from student where age != 20;
select * from student where age <> 20;

-- 查询年龄大于等于20，小于等于30
select * from student where age >= 20 and age <= 30;
select * from student where age between 20 and 30;

-- 查询年龄22岁，18岁，25岁的信息
select * from student where age = 22 or age = 18 or age = 25;
select * from student where age in(22, 18, 25);

-- 查询英语成绩为null
select * from student where english is null;

-- 查询英语成绩不为null
select * from student where english is not null;

-- 查询姓马的有哪些？
select * from student where name like '马%';

-- 查询姓名第二个字是化的人
select * from student where name like '_化%';

-- 查询姓名是3个字的人
select * from student where name like '___';

-- 查询姓名中包含德的人
select * from student where name like '%德%';
```

##### 5.4 排序查询

```sql
order by 排序字段1 排序方式1，排序字段2 排序方式2 ...

排序方式:
ASC：升序，默认的。
DESC：降序

注意:
如果有多个排序条件，则当前边的条件值一样时，才会判断第二条件。
```

##### 5.5 聚合函数

```sql
count: 计算个数
	1. 一般选择非空的列:主键
	2. count(*)
max: 计算最大值
min: 计算最小值
sum: 计算和
avg: 计算平均值

注意:聚合函数的计算，排除null值
解决方案：
1.选择不包含非空的列进行计算
2.IFNULL函数
```

##### 5.6 分组查询

```sql
语法: group by 分组字段;
```

> where 和 having 的区别
>
> 1. where 在分组之前进行限定，如果不满足条件，则不参与分组
> 2. having在分组之后进行限定，如果不满足结果，则不会被查询出来
>
> 2. where 后不可以跟聚合函数，having可以进行聚合函数的判断

```sql
-- 按照性别分组。分别查询男、女同学的平均分
select sex, avg(math) from student group by sex;

-- 按照性别分组。分别查询男、女同学的平均分,人数
select sex, avg(math), count(id) from student group by sex;

-- 按照性别分组。分别查询男、女同学的平均分,人数 要求:分数低于70分的人，不参与分组
select sex, avg(math), count(id) from student where math > 70 group by sex;

-- 按照性别分组。分别查询男、女同学的平均分,人数 要求:分数低于70分的人，不参与分组,分组之后。人数要大于2个人
select sex, avg(math), count(id) from student where math > 70 group by sex having count(id) > 2;

```

##### 5.7 分页查询

```sql
语法：limit 开始的索引,每页查询的条数;
公式：开始的索引 = （当前的页码 - 1） * 每页显示的条数

-- 每页显示3条记录 
SELECT * FROM student LIMIT 0,3; -- 第1页		
SELECT * FROM student LIMIT 3,3; -- 第2页		
SELECT * FROM student LIMIT 6,3; -- 第3页
```

##### 5.8 多表查询

> 笛卡尔积：
> 	有两个集合A，B 。取这两个集合的所有组成情况。
> 	要完成多表查询，需要消除无用的数据

​	<img src="C:\Users\wang1\Desktop\mdSource\数据库\1578034952739.png" style="zoom:67%;" />

​	<img src="C:\Users\wang1\Desktop\mdSource\数据库\1578034912661.png" style="zoom:67%;" />

- 内连接查询

  - 隐式内连接

    ```sql
    -- 查询所有员工信息和对应的部门信息
    SELECT * 
    FROM 
    	emp,
    	dept
    where 
    	emp.`dept_id` = dept.`id`;
    	
    -- 查询员工表的名称，性别。部门表的名称
    SELECT 
    	emp.name,
    	emp.gender, 
    	dept.name
    FROM 
    	dept,
    	emp
    WHERE 
    	emp.dept_id = dept.id
    	
    	
    SELECT 
    	t1.name,
    	t1.gender,
    	t2.name 
    FROM
    	emp t1,
    	dept t2
    WHERE 
    	t1.`dept_id` = t2.`id`;
    ```

  - 显式内连接

    ```sql
    /*
    	select 字段列表 from 表名1 [inner] join 表名2 on 条件
    */
    
    -- 查询所有员工信息和对应的部门信息
    SELECT * 
    FROM 
    	emp
    JOIN 
    	dept
    ON 
    	emp.dept_id = dept.id;
    
    ```

- 外连接查询

  - 左外连接

    ```sql
    /*
    	查询的是左表所有数据以及其交集部分
    	select 字段列表 from 表1 left [outer] join 表2 on 条件；
    */
    
    -- 查询所有员工信息，如果员工有部门，则查询部门名称；没有部门则不显示部门名称
    SELECT * 
    FROM 
    	emp
    LEFT JOIN
    	dept
    ON 
    	emp.dept_id = dept.id;
    ```

    ​	<img src="C:\Users\wang1\Desktop\mdSource\数据库\1578036941079.png" style="zoom: 67%;" />

  - 右外连接

    ```sql
    /*
    	查询的是右表所有数据以及其交集部分
    	select 字段列表 from 表1 right [outer] join 表2 on 条件；
    */
    
    --查询所有部门信息，入伙部门有员工，则查询员工信息；没有员工信息则不显示员工信息
    SELECT * 
    FROM 
    	emp
    RIGHT JOIN
    	dept
    ON 
    	emp.dept_id = dept.id;
    ```

    ​	<img src="C:\Users\wang1\Desktop\mdSource\数据库\1578036846244.png" style="zoom: 67%;" />

- 子查询

  ```sql
  /*
  	概念：查询中的嵌套查询 
  */
  
  -- 查询最高的工资是多少 
  select max(salary) from emp;
  -- 查询员工信息，并且工资等于9000的
  select * from emp where salary = 9000;
  -- 条sql语句整合
  select * 
  from 
  	emp 
  where 
  	emp.salary =(SELECT max(salary) FROM emp);
  	
  ```

  

#### 第六章 约束

##### 6.1 概念

​	对表中的数据进行限定，保证数据的正确性、有效性和完整性。

##### 6.2 分类

- 主键约束：primary key
- 外键约束：foreign key
- 唯一约束：unique
- 非空约束：not null

##### 6.3 主键约束

```sql
/*
	1. 含义：非空且唯一
	2. 一张表只能有一个字段为主键
	3. 主键就是表中记录的唯一标识
*/

-- 创建表时，添加主键约束
create table student(
    id int primary key,
   	name varchar(20)
);
-- 删除主键
alter table student drop primary key;
-- 创建表后，添加主键
alter table student modify id int primary key;

/*
	如果某一列是数值类型的，使用 auto_increment 可以来完成值得自动增长
*/
-- 自动增长
create table student(
	id int primary key auto_increment,
    name varchar(20)
);
-- 创建表后，删除自动增长
alter table student modify id int;
-- 创建表后，增加自动增长
alter table student modify id int auto_increment;
```

##### 6.4 外键约束

```sql
/*
	概念：让表与表产生关系，从而保证数据的正确性。
*/
-- 创建表时，添加外键
create table 表名(
	...
    外键列 
    [constraint] [外键名称] foreign key (外键名称) 
    references 主表名称(主表列名称)
);

-- 删除外键 
alter table 表名 drop foreign key 外键名称;

-- 创建表后，添加外键
alter table 表名 add [constraint] [外键名称] foreign key (外键字段名称) 
references 主表名称(主表列名称) 

-- 级联操作
/*
	级联更新：ON UPDATE CASCADE 
	级联删除：ON DELETE CASCADE
*/

-- 添加级联操作
alter table 表名 [constraint] [外键名称] foreign key (外键字段名称) references 主表名称(主表列名称) on update cascade on delete cascade;

```

##### 6.5 唯一约束

```sql
-- 创建表时，添加唯一约束,值不能重复
CREATE TABLE stu(
 	id INT,
    phone_number VARCHAR(20) UNIQUE -- 添加了唯一约束
);
注意:
mysql中，唯一约束限定的列的值可以有多个null

-- 删除唯一约束
alter table stu drop index phone_number;

-- 创建表后，添加唯一约束
alter table stu modify phone_nubmer varchar(20) unique;
```

##### 6.6 非空约束

```sql
-- 创建表时添加约束
create table stu(
	id int,
    name varchar(20) not null
);

--创建表后，添加非空约束
alter table stu modify name varchar(20) not null;

--删除name的非空约束
alter table stu modify name vachar(20);
```

#### 第七章 数据库设计

##### 7.1 多表之间关系

- 一对一

  如人和身份证

  > 实现方式：可以在任意一方添加唯一外键指向另一方的主键。

- 一对多（多对一）

  如部门和员工，一个部门可以有多个员工，一个员工只能对应一个部门

  > 实现方式：在多的一方建立外键，指向少的一方的主键。

- 多对多

  如学生和课程，一个学生可以选择很多们课程，一个课程也可以被很多学生选择

  > 实现方式：多对多关系实现需要借助第三张中间表。中间表至少包含两个字段，这两个字段作为第三张表的外键，分别指向两张表的主键。

##### 7.2 范式

- 概念

  设计数据库时，需要遵循的一些规范。要遵循后边的范式要求，必须先遵循前边的所有范式要求

- 分类

  第一范式（1NF）：每一列都是不可分割的原子数据项

  第二范式（2NF）：在1NF的基础上，非码属性必须完全依赖于码（在1NF基础上消除非主属性对主码的部分函数依赖）

  > 函数依赖：A-->B,如果通过A属性(属性组)的值，可以确定唯一B属性的值。则称B依赖于A
  >    例如：学号-->姓名。  （学号，课程名称） -->  分数
  >
  > 完全函数依赖：A-->B， 如果A是一个属性组，则B属性值得确定需要依赖于A属性组中所有的属性值。
  > 	例如：（学号，课程名称） --> 分数
  >
  > 
  >
  > 部分函数依赖：A-->B， 如果A是一个属性组，则B属性值得确定只需要依赖于A属性组中某一些值即可。
  > 	例如：（学号，课程名称） -- > 姓名
  >
  > 码：如果在一张表中，一个属性或属性组，被其他所有属性所完全依赖，则称这个属性(属性组)为该表的码
  > 	例如：该表中码为：（学号，课程名称）
  >
  > 主属性：码属性组中的所有属性
  >
  > 非主属性：除过码属性组的属性

- 第三范式（3NF）：在2NF基础上，任何非主属性不依赖于其它非主属性（在2NF基础上消除传递依赖）

  > 传递函数依赖：A-->B, B -- >C . 如果通过A属性(属性组)的值，可以确定唯一B属性的值，在通过B属性（属性组）的值可以确定唯一C属性的值，则称 C 传递函数依赖于A
  > 	例如：学号-->班级名，班级名-->班主任



#### 第八章 数据库的备份和还原

- 命令行

  ```sql
  -- 备份数据库
  mysqldump -u 用户名 -p 密码 [-B] 数据库名称 > 备份路径 
  -- -B指令备份的数据库中自带创建数据库的指令，恢复时更加的方便。
  
  -- 仅备份数据库的某几张表
  mysqldump -u 用户名 -p 密码 [-B] 数据库名称 表1 表2 > 备份路径
  
  -- 还原
  1. 登录数据库
  2. 创建数据库
  ```
3. 使用数据库
  4. 执行文件: source 文件路径
  ```
  

- 图形化工具

### 第九章 事务

##### 9.1 概念

如果一个包含多个步骤的业务操作被事务管理，那么这些操作要么同时成功，要么同时失败。

##### 9.2 操作 

​```sql
-- 开启事务
start transaction;
-- 提交
commit
-- 回滚
rollback;
  ```

##### 9.3 案例

##### 9.4 事务提交方式

- 自动提交

  > mysql就是自动提交的，一条DML(增删改)语句会自动提交一次事务

- 手动提交

  > Oracle 数据库默认是手动提交事务，需要先开启事务，再提交

##### 9.5 事务的特征

- 原子性：是不可分割的最小操作单位，要么同时成功，要么同时失败。
- 持久性：当事务提交或回滚后，数据库会持久化的保存数据。
- 隔离性：多个事务之间。相互独立。
- 一致性：事务操作前后，数据总量不变

##### 9.6 事务的隔离级别

- 概念：多个事务之间隔离的，相互独立的。但是如果多个事务操作同一批数据，则会引发一些问题，设置不同的隔离级别就可以解决这些问题。

- 存在问题

  > 脏读：一个事务，读取到另一个事务中没有提交的数据
  >
  > 不可重复读（虚读）：在同一个事务中，两次读取到的数据不一样
  >
  > 幻读：一个事务操作(DML)数据表中所有记录，另一个事务添加了一条数据，则第一个事务查询不到自己的修改

- 隔离级别

  > read uncommitted：读未提交               		产生的问题：脏读、不可重复读、幻读		
  >
  > read committed：读已提交 （Oracle）  	 产生的问题：不可重复读、幻读
  >
  > repeatable read：重复读 （MySQL默认）   产生的问题：幻读
  >
  > serializable:  串行化                 						可以解决所有的问题
  >
  > 
  >
  > 隔离级别从小到大安全性越来越高，但是效率越来越低

  ```sql
  -- 数据库查询隔离级别
  select @@tx_isolation;
  
  -- 数据库设置隔离级别
  set global transaction isolation level  级别字符串;
  ```

  

### 第十章 DCL管理授权用户

##### 10.1 用户管理

```sql
-- 添加用户
create user 'user'@'host' identified by 'password';

-- 删除用户
drop user 'user'@'host';

-- 修改用户密码
update user set password = password('新密码') where user = 'user';
set password for 'user'@'host' = password('新密码');
-- 查询用户
use mysql;
select * from user;
```

##### 10.2 权限管理

```sql
-- 查询权限
show grants for 'user'@'host';
show grants for 'user'@'%';

-- 授予权限
grant 权限列表 on 数据库名.表名 to 'user'@'host';

-- 撤销权限
revoke 权限列表 on 数据库名.表名 to 'user'@'host';
```

> 通配符： % 表示可以在任意主机使用用户登录数据库