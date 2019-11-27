# 概述

[MySQL5.5安装](https://blog.csdn.net/flyfrommath/article/details/78714121)

[MySQL安装与配置详解](https://www.cnblogs.com/sshoub/p/4321640.html)

用户名密码：默认都设为root

启动MySQL服务：net start mysql

停止MySQL服务：net stop mysql

输出用户名密码：mysql -u用户名 -p密码

DBA：数据库管理员

数据库（DataBase/DB）：存储和管理数据的仓库

数据库的特点

1. 持久化存储数据，数据库实际上是一个文件系统
2. 方便存储和管理数据使用
3. 使用统一的方式操作数据库 -- SQL



mysql卸载        

1. 去mysql的安装目录找到my.ini文件
   * 复制 datadir="C:/ProgramData/MySQL/MySQL Server 5.5/Data/"        

2. 卸载MySQL
3. 删除C:/ProgramData目录下的MySQL文件夹



MySQL服务启动

1. 手动
2. cmd--> services.msc 打开服务的窗口
3. 使用管理员权限打开cmd
   * net start mysql : 启动mysql的服务
   * net stop mysql:关闭mysql服务



MySQL登录

1. mysql -uroot -p密码
2. mysql -hip -uroot -p连接目标的密码
3. mysql --host=ip --user=root --password=连接目标的密码

MySQL退出

1. exit
2. quit



MySQL目录结构

1. MySQL安装目录：basedir="D:/develop/MySQL/"

   * 配置文件 my.ini

2. MySQL数据目录：datadir="C:/ProgramData/MySQL/MySQL Server 5.5/Data/"

   * 数据库：文件夹

      * 表：文件
      * 数据：数据



数据库的备份和还原

1. 命令行
   * 备份： mysqldump -u用户名 -p密码 数据库名称 > 保存的路径
   * 还原：1.登录数据库；2.创建数据库；3.使用数据库；4.执行文件，source 文件路径

2. 图形化工具：sqlyog



# SQL

SQL：Structured Query Language，结构化查询语言。定义了操作所有关系型数据库的规则。每一种数据库都是基于SQL，但各有所不同

SQL通用语法

1. SQL 语句可以单行或多行书写，以分号结尾
2. 可使用空格和缩进来增强语句的可读性
3. 不区分大小写，关键字建议使用大写
4. 三种注释
   * 单行注释：-- 注释内容 或 # 注释内容(mysql 特有)
   * 多行注释：/* 注释 */

SQL分类

1. DDL(Data Definition Language)：数据定义语言
   操作数据库、表、字段。关键字：create, drop,alter 等
2.  DML(Data Manipulation Language)：数据操作语言
    对数据库中的数据增删改。关键字：insert, delete, update 等
3. DQL(Data Query Language)：数据查询语言
   查询数据库中表的(数据)。关键字：select, where 等
4. DCL(Data Control Language)：数据控制语言(了解)
   定义数据库的访问权限和安全级别，及创建用户。关键字：GRANT， REVOKE 等



# DDL

DDL（数据库定义语言）：操作数据库、表

## 操作数据库

### 1.创建（Create）

创建数据库：create database 数据库名称;    

创建数据库，判断不存在，再创建：create database if not exists 数据库名称;    

创建数据库，并指定字符集：create database 数据库名称 character set 字符集名;

练习： 创建db4数据库，判断是否存在，并制定字符集为gbk        

```
 create database if not exists db4 character set gbk;
```

### 2.查询（Retrieve）

查询所有数据库的名称：show databases;    

查询某个数据库的字符集/查询某个数据库的创建语句：show create database 数据库名称;

4个MySQL自带的库

* information_schema
* mysql
* test：空数据库，一般不用
* performance_schema：性能提升

### 3.修改（Update）

修改数据库的字符集：alter database 数据库名称 character set 字符集名称;

### 4.删除（Delete）

删除数据库：drop database 数据库名称;

判断数据库存在，存在再删除：drop database if exists 数据库名称;

### 5.使用

查询当前正在使用的数据库名称：select database();

使用数据库：use 数据库名称;



## 操作表

### 1.创建(Create)

1. create

```
create table 表名(
	列名1 数据类型1,
	列名2 数据类型2,
    ....
    列名n 数据类型n
);
```

注意：最后一列，不需要加逗号（,）

```
# 创建表案例
create table student (
    id int,
    name varchar(32),
    age int ,
    score double(4,1),
    birthday date,
    insert_time timestamp
);       
```

2. 复制表：create table 表名 like 被复制的表名;   

### 2.查询（Retrieve）

查询某个数据库中所有的表名称：show tables;            

查询表结构：desc 表名;

### 3. 修改（Update）

对表的修改都以 alter table 开头

1. 修改表名：alter table 旧表名 rename to 新表名;

2. 修改表的字符集：alter table 表名 character set 字符集名称;

3. 添加一列：alter table 表名 add 列名 数据类型;

4. 修改列名称、类型
   alter table 表名 change 列名 新列名 新数据类型;
   alter table 表名 modify 列名 新数据类型;

5. 删除列：alter table 表名 drop 列名;

### 4.删除（Delete）

drop table 表名;        

drop table  if exists 表名 ;



## 数据类型

1. int：整数类型，如age int,
2. double：小数类型
   * score double(5,2)：总共5位，其中2位小数

3. date：日期，只包含年月日，yyyy-MM-dd
4. datetime：日期，包含年月日时分秒，yyyy-MM-dd HH:mm:ss
5. timestamp：时间戳类型，包含年月日时分秒，yyyy-MM-dd HH:mm:ss    
   * 如果不主动赋值，或赋值为null，则默认使用当前的系统时间
6. varchar：字符串
   * name varchar(20)：姓名最大20个字符
   * zhangsan——8个字符，张三——2个字符



# DML

DML（数据库操作语言）：增删改表中数据

### 1.添加

语法：insert into 表名(列名1,列名2,...列名n) values (值1,值2,...值n);

注意

1. 列名和值要一一对应；
2. 如果表名后不定义列名，则默认给所有列添加值：insert into 表名 values(值1,值2,...值n);
3. 除了数字类型，其他类型需要使用引号（单双都可以）引起来

### 2.删除

语法：delete from 表名 [where 条件]

注意：

1. 如果不加条件，则删除表中所有记录。
2. 如果要删除所有记录
   * delete from 表名; -- 不推荐使用，n 记录会执行 n 次删除操作（麻烦）
   * TRUNCATE TABLE 表名; -- 推荐使用，效率更高；先删除表，再创建一张一样的表

### 3.修改

语法：update 表名 set 列名1 = 值1, 列名2 = 值2,... [where 条件];

注意：如果不加任何条件，则会将表中所有记录全部修改。



# DQL

DQL（数据库查询语句）：查询表中的记录

查询表中的记录：select * from 表名;
语法：
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

## 1.基础查询

1. 多个字段查询：select 字段名1，字段名2... from 表名；
   * 查询所有字段：select * from 表名;

2. 去重：distinct

3. 计算列

   * 使用四则运算计算一些列的值（一般只会进行数值型的计算）

   * ifnull(表达式1, 表达式2)：null参与的运算，计算结果都为null
     * 表达式1：哪个字段需要判断是否为null
     * null后的替换值

4. 起别名：as（as可以省略）



## 2.条件查询

条件查询：where子句后跟条件

* < 、>、<= 、>= 、= 、<>
* BETWEEN...AND  
* IN(集合)
*  LIKE：模糊查询
* 占位符
  _：单个任意字符
  %：多个任意字符
* IS NULL：判空
* and  或 &&
* or  或 ||
* not  或 !

```
-- 查询年龄大于20岁
SELECT * FROM student WHERE age > 20;

-- 查询年龄等于20岁
SELECT * FROM student WHERE age = 20;

-- 查询年龄不等于20岁
SELECT * FROM student WHERE age <> 20;

-- 查询年龄大于等于20 小于等于30
SELECT * FROM student WHERE age >= 20 &&  age <=30;
SELECT * FROM student WHERE age >= 20 AND  age <=30;
SELECT * FROM student WHERE age BETWEEN 20 AND 30;

-- 查询年龄22岁，18岁，25岁的信息
SELECT * FROM student WHERE age = 22 OR age = 18 OR age = 25
SELECT * FROM student WHERE age IN (22,18,25);

-- 查询英语成绩为null
SELECT * FROM student WHERE english = NULL; -- 不对的。null值不能使用=或<>判断
SELECT * FROM student WHERE english IS NULL;

-- 查询英语成绩不为null
SELECT * FROM student WHERE english  IS NOT NULL;
    
-- 查询姓马的学生
SELECT * FROM student WHERE NAME LIKE '马%';

-- 查询姓名第二个字是化的人
SELECT * FROM student WHERE NAME LIKE "_化%";

-- 查询姓名是3个字的人
SELECT * FROM student WHERE NAME LIKE '___';

-- 查询姓名中包含德的人
SELECT * FROM student WHERE NAME LIKE '%德%';
```



## 3.排序查询

语法：order by 子句，即 order by 排序字段1 排序方式1 ，  排序字段2 排序方式2...

排序方式
* ASC：升序（默认）
* DESC：降序

注意：如果有多个排序条件，则当前边的条件值一样时，才会判断第二条件



## 4.聚合函数

定义：将一列数据作为一个整体，进行纵向的计算

语法：

1. count：计算个数
   
    一般选择非空的列：主键
    
    count(*)：计算总数
    
2. max：计算最大值

3. min：计算最小值

4. sum：计算和

5. avg：计算平均值



注意：聚合函数的计算，排除null值。解决方法如下：

1. 选择不包含非空的列进行计算
2. 使用IFNULL函数



## 5.分组查询

语法：group by 分组字段；

分组之后查询的字段可以是：分组字段、聚合函数

where 和 having 的区别？
 	1. where 在分组之前进行限定，如果不满足条件，则不参与分组。having在分组之后进行限定，如果不满足结果，则不会被查询出来
 	2. where 后面不可以跟聚合函数，having后面可以进行聚合函数的判断

```sql
-- 按照性别分组。分别查询男、女同学的平均分
SELECT sex, AVG(math) FROM student GROUP BY sex;

-- 按照性别分组。分别查询男、女同学的平均分,人数
SELECT sex, AVG(math), COUNT(id) FROM student GROUP BY sex;

--  按照性别分组。分别查询男、女同学的平均分,人数 要求：分数低于70分的人，不参与分组
SELECT sex , AVG(math),COUNT(id) FROM student WHERE math >= 70 GROUP BY sex;


--  按照性别分组。分别查询男、女同学的平均分,人数 要求：分数低于70分的人，不参与分组,分组之后。人数要大于2个人
SELECT sex , AVG(math),COUNT(id) FROM student WHERE math >= 70 GROUP BY sex HAVING COUNT(id) > 2;
SELECT sex , AVG(math),COUNT(id) 人数 FROM student WHERE math > 70 GROUP BY sex HAVING 人数 > 2;
```



## 6.分页查询

语法：limit 开始的索引, 每页查询的条数;

公式：开始的索引 = （当前页码 - 1） * 每页条数

limit是mysql独有的SQL语句

```sql
-- 每页显示3条记录
SELECT * FROM student LIMIT 0,3; -- 第1页
SELECT * FROM student LIMIT 3,3; -- 第2页
SELECT * FROM student LIMIT 6,3; -- 第3页
```



# DCL

DCL（数据库控制语句）：管理用户和授权，一般由数据库管理人员（DBA）使用

## 1.管理用户

添加用户：CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';

删除用户：DROP USER '用户名'@'主机名';

修改用户密码

1. UPDATE USER SET PASSWORD = PASSWORD('新密码') WHERE USER = '用户名';

```
UPDATE USER SET PASSWORD = PASSWORD('abc') WHERE USER = 'lisi';
```

2. SET PASSWORD FOR '用户名'@'主机名' = PASSWORD('新密码');

```
 SET PASSWORD FOR 'root'@'localhost' = PASSWORD('123');
```



MySQL忘记用户密码

1. cmd右键以管理员身份运行 → net stop mysql 停止mysql服务
2. 使用无验证方式启动mysql服务： mysqld --skip-grant-tables，回车
3. 打开新的cmd窗口，输入以下mysql命令
    use mysql;
    update user set password = password('你的新密码') where user = 'root';
4. 关闭两个窗口
5. 打开任务管理器，手动结束mysqld.exe 的进程
6. 启动mysql服务
7. 使用新密码登录



查询用户

1. 切换到mysql数据库：USE myql;
2. 查询user表：SELECT * FROM USER;



## 2.权限管理

1. 查询权限：SHOW GRANTS FOR '用户名'@'主机名';

   例：SHOW GRANTS FOR 'lisi'@'%';

2. 授予权限：grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';

   给张三用户授予所有权限，在任意数据库任意表上：GRANT ALL ON *.* TO 'zhangsan'@'localhost';    

   通配符 *.* 表示所有数据库所有表

3. 撤销权限：revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';
   例：REVOKE UPDATE ON db3.account FROM 'lisi'@'%';        

   通配符 % 表示可以在任意主机使用用户登录数据库



# 约束

约束：对表中的数据进行限定，保证数据的正确性、有效性和完整性。    

分类

1. 主键约束：primary key
2. 非空约束：not null        
3. 唯一约束：unique        
4. 外键约束：foreign key

## 主键约束

语法：primary key

主键必须非空且唯一：一张表只能有一个字段为主键，主键就是表中记录的唯一标识

添加主键

1. 创建表时，添加主键约束

   ```
   create table stu(
       id int primary key,-- 给id添加主键约束
       name varchar(20)
   );
   ```

2. 创建完表后，添加主键：ALTER TABLE stu MODIFY id INT PRIMARY KEY;

3. 删除主键：ALTER TABLE stu DROP PRIMARY KEY;

4. 自增长

   * 如果某一列是数值类型的，使用 auto_increment 可以来完成自动增长

   * 创建表时添加主键约束，完成自增长

     ```
     create table stu(
         id int primary key auto_increment,-- 给id添加主键约束
         name varchar(20)
     );
     ```

   * 创建表后，添加自增长：ALTER TABLE stu MODIFY id INT AUTO_INCREMENT;
   * 删除自增长： ALTER TABLE stu MODIFY id INT;



## 非空约束

语法：not null，值不能为null

1. 创建表时，添加非空约束

   ```
   CREATE TABLE stu(
   	id INT,
   	NAME VARCHAR(20) NOT NULL -- name为非空
   );
   ```

2. 创建完表后，添加非空约束：ALTER TABLE stu MODIFY NAME VARCHAR(20) NOT NULL;

3. 删除name字段的非空约束：ALTER TABLE stu MODIFY NAME VARCHAR(20);



## 唯一约束

语法：unique，值不能重复

1. 创建表时，添加唯一约束

   ```
   CREATE TABLE stu(
   	id INT,
   	phone_number VARCHAR(20) UNIQUE -- 添加唯一约束
   );
   ```

2. 创建完表后，添加唯一约束： ALTER TABLE stu MODIFY phone_number VARCHAR(20) UNIQUE;

3. 删除唯一约束：ALTER TABLE stu DROP INDEX phone_number;



## 外键约束

语法：foreign key，让表与表产生关系，从而保证数据的正确性

1. 创建表时，添加外键

   ```
   create table 表名(
       ....
       外键列
       constraint 外键名称 foreign key (外键列名称) references 主表名称(主表列名称)
   );
   ```

2. 创建完表后，添加外键： ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列名称);

3. 删除外键：ALTER TABLE 表名 DROP 外键名称

4. 级联操作：

   * 添加级联操作：ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列名称) ON UPDATE CASCADE ON DELETE CASCADE  ;
   * 分类：
     * 级联更新：ON UPDATE CASCADE            
     * 级联删除：ON DELETE CASCADE



# 多表查询

## 多表关系

多表之间的关系有：一对一，一对多，多对一，多对多

实现

* 一对一：在任意一方添加唯一外键指向另一方的主键
* 一对多(多对一)：在多的一方建立外键，指向一的一方的主键
* 多对多：借助第三张中间表。中间表至少包含两个字段，这两个字段作为第三张表的外键，分别指向两张表的主键

指向：关键字 REFERENCES

案例

```
-- 创建旅游线路分类表 tab_category
-- cid 旅游线路分类主键，自动增长
-- cname 旅游线路分类名称非空，唯一，字符串 100
CREATE TABLE tab_category (
	cid INT PRIMARY KEY AUTO_INCREMENT,
    cname VARCHAR(100) NOT NULL UNIQUE
);
```

```
-- 创建旅游线路表 tab_route
/*
rid 旅游线路主键，自动增长
rname 旅游线路名称非空，唯一，字符串 100
price 价格
rdate 上架时间，日期类型
cid 外键，所属分类
*/
CREATE TABLE tab_route(
	rid INT PRIMARY KEY AUTO_INCREMENT,
    rname VARCHAR(100) NOT NULL UNIQUE,
    price DOUBLE,
    rdate DATE,
    cid INT,
    FOREIGN KEY (cid) REFERENCES tab_category(cid)
);            
```

```
/*
创建收藏表 tab_favorite
rid 旅游线路 id，外键
date 收藏时间
uid 用户 id，外键
rid 和 uid 不能重复，设置复合主键，同一个用户不能收藏同一个线路两次
*/
CREATE TABLE tab_favorite (
	rid INT, -- 线路id
    DATE DATETIME,
    uid INT, -- 用户id
    -- 创建复合主键
    PRIMARY KEY(rid,uid), -- 联合主键
    FOREIGN KEY (rid) REFERENCES tab_route(rid),
    FOREIGN KEY(uid) REFERENCES tab_user(uid)
);
```



## 多表查询

语法

```
select
	列名列表
from
	表名列表
where....
```



创建表

```
# 创建部门表
CREATE TABLE dept(
	id INT PRIMARY KEY AUTO_INCREMENT,
    NAME VARCHAR(20)
);
INSERT INTO dept (NAME) VALUES ('开发部'),('市场部'),('财务部');
# 创建员工表
CREATE TABLE emp (
	id INT PRIMARY KEY AUTO_INCREMENT,
    NAME VARCHAR(10),
    gender CHAR(1), -- 性别
    salary DOUBLE, -- 工资
    join_date DATE, -- 入职日期
    dept_id INT,
    FOREIGN KEY (dept_id) REFERENCES dept(id) -- 外键，关联部门表(部门表的主键)
);
INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('孙悟空','男',7200,'2013-02-24',1);
INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('猪八戒','男',3600,'2010-12-02',2);
INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('唐僧','男',9000,'2008-08-08',2);
INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('白骨精','女',5000,'2015-10-07',3);
INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('蜘蛛精','女',4500,'2011-03-14',1);
```



## 笛卡尔积

笛卡尔积：有两个集合A和B，取这两个集合的所有组合情况。

但是要完成多表查询，需要消除无用的数据。



## 多表查询的分类

### 内连接查询

1. 隐式内连接：使用where条件消除无用数据

   ```
   -- 查询所有员工信息和对应的部门信息
   SELECT * 
   FROM 
   	emp,
   	dept 
   WHERE 
   	emp.`dept_id` = dept.`id`;
   
   -- 查询员工表的名称，性别；部门表的名称
   -- 方法一
   SELECT 
   	emp.name,
   	emp.gender,
   	dept.name 
   FROM 
   	emp,
   	dept 
   WHERE 
   	emp.`dept_id` = dept.`id`;
   
   -- 方法二
   -- 起别名
   SELECT
   	t1.name, -- 员工表的姓名
   	t1.gender,-- 员工表的性别
   	t2.name -- 部门表的名称
   FROM
       emp t1,
       dept t2
   WHERE
       t1.`dept_id` = t2.`id`;
   ```

   

2. 显式内连接：join···on···
   语法： select 字段列表 from 表名1 [inner] join 表名2 on 条件
   例如：

   ​		SELECT * FROM emp INNER JOIN dept ON emp.`dept_id` = dept.`id`;    

   ​		SELECT * FROM emp JOIN dept ON emp.`dept_id` = dept.`id`;    



### 外连接查询



外连接查询：以某一张表为基表，进行相关查询。取出基表的所有记录，然后逐条与另一张表连接，
不管能不能匹配，基表最终都会保留。能匹配，正确保留；不能匹配，其他表的字段都置空null。



外连接查询分类

1. 左外连接

   语法：select 字段列表 from 表1 left [outer] join 表2 on 条件；                

   查询的是左表所有数据以及两表交集部分     

   ```
   -- 查询所有员工信息，如果员工有部门，则查询部门名称，没有部门，则不显示部门名称                    SELECT     
   	t1.*,
   	t2.`name` 
   FROM 
   	emp t1 
   LEFT JOIN 
   	dept t2 
   ON t1.`dept_id` = t2.`id`;
   ```

   

2. 右外连接

   语法：select 字段列表 from 表1 right [outer] join 表2 on 条件；                

   查询的是右表所有数据以及其交集部分。

   ```
   SELECT 
   	* 
   FROM 
   	dept t2 
   RIGHT JOIN 
   	emp t1 
   ON 
   	t1.`dept_id` = t2.`id`;     
   ```

3. 子查询

   ```
   -- 查询工资最高的员工信息
   
   -- 方法1：两步走
   -- 1 查询最高的工资是多少 9000
   SELECT MAX(salary) FROM emp;
                   
   -- 2 查询员工信息，并且工资等于9000的
   SELECT * FROM emp WHERE emp.`salary` = 9000;
                   
   -- 方法2：一条sql就完成这个操作：——子查询
   SELECT * FROM emp WHERE emp.`salary` = (SELECT MAX(salary) FROM emp);
   ```

   子查询分类

   1. 子查询的结果是单行单列

      子查询可以作为条件，使用运算符去判断。 运算符： > >= < <= =

      ```
      -- 查询员工工资小于平均工资的人
      SELECT * FROM emp WHERE emp.salary < (SELECT AVG(salary) FROM emp);
      ```

   2. 子查询的结果是多行单列                   

      子查询可以作为条件，使用运算符in来判断

      ```
      -- 查询'财务部'和'市场部'所有的员工信息
      -- 方法1
      SELECT id FROM dept WHERE NAME = '财务部' OR NAME = '市场部';
      SELECT * FROM emp WHERE dept_id = 3 OR dept_id = 2;
      
      -- 方法2：子查询
      SELECT * FROM emp WHERE dept_id IN (SELECT id FROM dept WHERE NAME = '财务部' OR NAME = '市场部');
      ```

   3. 子查询的结果是多行多列的

      子查询可以作为一张虚拟表参与查询

      ```
      -- 查询员工入职日期是2011-11-11日之后的员工信息和部门信息
      -- 子查询
      SELECT * FROM dept t1 ,(SELECT * FROM emp WHERE emp.`join_date` > '2011-11-11') t2
      WHERE t1.id = t2.dept_id;
                          
      -- 普通内连接
      SELECT * FROM emp t1,dept t2 WHERE t1.`dept_id` = t2.`id` AND t1.`join_date` >  '2011-11-11'
      ```

      

