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

DDL（数据库操作语言）：操作数据库、表

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

