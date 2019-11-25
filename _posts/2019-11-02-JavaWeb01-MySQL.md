---
layout:     post                   
title:      JavaWeb01-MySQL基础
subtitle:   1. 数据库的概念2. 常见的数据库软件3. MySQL数据库的安装卸载4. MySQL数据库的登录退出5. MySQL的目录结构6. SQL语句的分类7. 数据库和数据表的操作8. 数据的添加(insert)9. 数据的删除(delete)10. 数据的修改(update)11. 数据的查询(select)12. 数据的复杂查询13. 约束的使用14. 多表关系(一对一、一对多、多对多)15. 三大范式详解16. 数据库的还原和备份17. 多表查询操作18. 事务介绍19. 事务的隔离20. 数据库的用户管理和权限管理
date:       2019-11-02       
author:     HY                 
header-img: img/post-bg-2015.jpg
catalog: true                  
tags:                   
    - MySQL
---





# JavaWeb01：MySQL基础

## 概述

### 数据库的基本概念

1. 数据库的英文单词： DataBase 简称 ： DB
2. 什么数据库？
    * 用于存储和管理数据的仓库。
3. 数据库的特点：
    1. 持久化存储数据的。其实数据库就是一个文件系统
    2. 方便存储和管理数据
    3. 使用了统一的方式操作数据库 -- SQL
4. 常见的数据库软件
    * 参见《MySQL基础.pdf》

### MySQL数据库软件

1. 安装
    * 参见《MySQL基础.pdf》
    * 客户端图形化工具：SQLYog
2. 卸载
    1. 去mysql的安装目录找到my.ini文件
        * 复制 datadir="C:/ProgramData/MySQL/MySQL Server 5.5/Data/"
    2. 卸载MySQL
    3. 删除C:/ProgramData目录下的MySQL文件夹。

3. 配置
>安装的是MySQL的服务，默认是开机自动启动

* MySQL服务启动
    1. 手动。
    2. cmd--> services.msc 打开服务的窗口
    3. 使用**管理员**打开cmd
        * `net start mysql` : 启动mysql的服务
        * `net stop mysql` : 关闭mysql服务
* MySQL登录
    1. mysql -u**root** -p**密码**
    2. mysql -h**ip地址** -u**root** -p**连接目标的密码**
    3. mysql --host=**ip地址** --user=**root** --password=**连接目标的密码**
* MySQL退出
    1. exit
    2. quit

* MySQL目录结构
    1. MySQL安装目录：basedir="D:/develop/MySQL/"
       
        * 配置文件 my.ini
    2. MySQL数据目录：datadir="D:\Program Files\MySQL\MySQL Server 5.5\data"
        * 几个概念
        * 数据库：文件夹
        * 表：文件
        * 数据：数据
        >MySQL数据库、表、数据的关系：
        >![](https://raw.githubusercontent.com/hihanying/FigureBed/master/img/MySQL数据库、表、数据的关系.bmp)

### SQL：DDL、DML、DQL

1. 什么是SQL？
    Structured Query Language：结构化查询语言
    其实就是定义了操作所有关系型数据库的规则。每一种数据库操作的方式存在不一样的地方，称为“方言”。
2. SQL通用语法
    1. SQL 语句可以单行或多行书写，以**分号**结尾。
    2. 可使用空格和缩进来增强语句的可读性。
    3. MySQL 数据库的 SQL 语句不区分大小写，关键字建议使用大写。
    4. 3 种注释
        * 单行注释: `-- 注释内容`（中间有空格）或 `#注释内容`
        * 多行注释: /* 注释 */

3. SQL分类
    1. DDL(Data Definition Language)数据定义语言
    用来定义**数据库对象：数据库，表，列等**。关键字：create, drop,alter 等
    2. DML(Data Manipulation Language)数据操作语言
    用来对数据库中**表的数据**进行增删改。关键字：insert, delete, update 等
    3. DQL(Data Query Language)数据查询语言
    用来查询数据库中**表的记录(数据)**。关键字：select, where 等
    4. DCL(Data Control Language)数据控制语言(了解)
    用来定义数据库的**访问权限和安全级别，及创建用户**。关键字：GRANT， REVOKE 等

## DDL：操作数据库、表

### 1. 操作数据库

1. C(Create):创建
    * 创建数据库：
        * create database 数据库名称;
    * 创建数据库，判断不存在，再创建：
        * create database if not exists 数据库名称;
    * 创建数据库，并指定字符集
        * create database 数据库名称 character set 字符集名;
    * 练习： 创建db4数据库，判断是否存在，并制定字符集为gbk
        * create database if not exists db4 character set gbk;
    
2. R(Retrieve)：查询
    * 查询所有数据库的名称:
        * show databases;
    * 查询某个数据库的字符集:查询某个数据库的创建语句
        * show create database 数据库名称;
    
3. U(Update):修改
    * 修改数据库的字符集
        * alter database 数据库名称 character set 字符集名称;
    
4. D(Delete):删除
    * 删除数据库
        * drop database 数据库名称;
    * 判断数据库存在，存在再删除
        * drop database if exists 数据库名称;
    
5. 使用数据库
    * 查询当前正在使用的数据库名称
        * select database();
    * 使用数据库
        * use 数据库名称;
    
### 2. 操作表

1. C(Create):创建
    * 语法：
        ```mysql
        create table 表名(
            列名1 数据类型1,
            ....,
            列名n 数据类型n
        );
        ```
        > **注意：**
        >
        > * 最后一列，不需要加逗号（,）
        > * 数据库类型：
        >     1. int：整数类型
        >         * age int,
        >     2. double:小数类型
        >         * score double(5,2)
        >     3. date:日期，只包含年月日，yyyy-MM-dd
        >     4. datetime:日期，包含年月日时分秒	 yyyy-MM-dd HH:mm:ss
        >     5. timestamp:时间戳类型	包含年月日时分秒	 yyyy-MM-dd HH:mm:ss	
        >         * 如果将来不给这个字段赋值，或赋值为null，则默认使用当前的系统时间，来自动赋值
        >     6. varchar：字符串
        >         * name varchar(20):姓名最大20个字符
        >           * zhangsan 8个字符  张三 2个字符

    * 创建表
        ```mysql
        create table student(
            id int,
            name varchar(32),
            age int ,
            score double(4,1),
            birthday date,
            insert_time timestamp
        );
        ```
    * 复制表：
        ```mysql
        create table 表名 like 被复制的表名;
        ```

2. R(Retrieve)：查询
    * 查询某个数据库中所有的表名称
        * show tables;
    * 查询表结构
        * desc 表名;
3. U(Update):修改
    1. 修改表名
        alter table 表名 rename to 新的表名;
    2. 修改表的字符集
        alter table 表名 character set 字符集名称;
    3. 添加一列
        alter table 表名 add 列名 数据类型;
    4. 修改列名称 类型
        alter table 表名 change 列名 新列名 新数据类型;
        alter table 表名 modify 列名 新数据类型;
    5. 删除列
        alter table 表名 drop 列名;
4. D(Delete):删除
    * drop table 表名;
    * drop table  if exists 表名;

## DML：增删改表中数据

1. 添加数据：
    * 语法：
      
        ```mysql
        insert into 表名(列名1,列名2,...列名n) values(值1,值2,...值n);
        ```
        
    * 注意：
        1. 列名和值要一一对应。
        
        2. 如果表名后，不定义列名，则默认给所有列添加值
           
            ```mysql
            insert into 表名 values(值1,值2,...值n);
            ```
            
        3. 除了数字类型，其他类型需要使用单/双引号，例如："1893-11-11"
2. 删除数据：
    * 语法：
      
        ```mysql
        delete from 表名 [where 条件]
        ```
        
    * 注意：
        1. 如果不加条件，则删除表中所有记录。
        
        2. 如果要删除所有记录
           
            ```mysql
            delete from 表名; -- 不推荐使用。有多少条记录就会执行多少次删除操作
            truncate table 表名; -- 推荐使用，效率更高，先删除表，然后再创建一张一样的表。
            ```
3. 修改数据：
    * 语法：
        ```mysql
                update 表名 set 列名1 = 值1, 列名2 = 值2,... [where 条件];
        ```
    * 注意：
      
        * 如果不加任何条件，则会将表中所有记录全部修改。

## DQL：查询表中的记录

> 主要的语法：
>     - select 字段列表
>         - from 表名列表
>         - where 条件列表
>         - group by 分组字段
>         - having 分组之后的条件
>         - order by 排序
>         - limit 分页限定

### 1. 基础查询
1. 多个字段的查询
   
    ```mysql
    select 字段名1，字段名2... from 表名;
    ```
    
    v注意：
    
    * 如果查询所有字段，则可以使用*来替代字段列表。
    
2. 去除重复：
   
    ```mysql
    select distinct 字段名 from 表名;
    select distinct 字段名1,字段名2... from 表名;-- 保证几个字段都相同才去重
    ```
    
3. 计算列
    * 一般可以使用四则运算计算一些列的值。（一般只会进行数值型的计算）
    
        ```mysql
        select 字段名1,字段名2+字段名3 from 表名;-- null参与的运算，计算结果都为null
    select 字段名1,字段名2+ifnull(字段名3,0) from 表名;
        ```
    
    * `ifnull(表达式1,表达式2)`：
      
        * 表达式1：哪个字段需要判断是否为null
        * 表达式2：如果该字段为null后的替换值。
    
4. 起别名：

    * as：as也可以省略
    
      ```mysql
      select 字段名1+ifnull(字段名2,0) as 新字段名 from 表名;
      ```

### 2. 条件查询：where

- 语法：

    ```mysql
    SELECT 字段名 FROM 表名 WHERE 条件; 
    ```

- 运算符

  - 比较运算符

    - <、>、<=、>=、=、<>：<>在 SQL 中表示不等于，在 mysql 中也可以使用`!=`，没有`== `

    ```mysql
    -- 查询年龄大于20岁
    SELECT * FROM student WHERE age > 20;
    SELECT * FROM student WHERE age >= 20;
    -- 查询年龄等于20岁
    SELECT * FROM student WHERE age = 20;
    -- 查询年龄不等于20岁
    SELECT * FROM student WHERE age != 20;
    SELECT * FROM student WHERE age <> 20;
    ```

  - 逻辑运算符

    - and 或 &&：**与**，SQL 中建议使用前者，后者并不通用。 
    - or 或 ||：**或** 
    - not 或 !：**非** 

    ```mysql
    -- 查询年龄大于等于20 小于等于30
    SELECT * FROM student WHERE age >= 20 &&  age <=30;
    SELECT * FROM student WHERE age >= 20 AND  age <=30;
    SELECT * FROM student WHERE age BETWEEN 20 AND 30;
    -- 查询年龄22岁，18岁，25岁的信息
    SELECT * FROM student WHERE age = 22 OR age = 18 OR age = 25
    ```

  - 其他条件

    - 范围查询 
      - `BETWEEN 值 1 AND 值 2`：表示从值 1 到值 2 范围，包头又包尾 
    - IN( 集合) 
      - 集合表示多个值，使用逗号分隔，集合里面的每个数据都会作为一次条件，只要满足条件的就会显示 
      - 语法：`SELECT 字段名 FROM 表名 WHERE 字段 in (数据 1, 数据 2...); `
    - like 关键字表示模糊查询
      - 语法：`SELECT * FROM 表名 WHERE 字段名 LIKE '通配符字符串'; `
      - MySQL 通配符
        - `%`：匹配任意多个字符串
        - `_`：匹配一个字符 
    - IS NULL：查询某一列为NULL 的值，注：不能写=NULL 

    ```mysql
    -- 查询年龄22岁，18岁，25岁的信息
    SELECT * FROM student WHERE age IN (22,18,25);
    -- 查询英语成绩为null
    SELECT * FROM student WHERE english IS NULL;
    -- 查询英语成绩不为null
    SELECT * FROM student WHERE english  IS NOT NULL;
    -- 查询姓马的有哪些？ like
    SELECT * FROM student WHERE NAME LIKE '马%';
    -- 查询姓名第二个字是化的人
    SELECT * FROM student WHERE NAME LIKE "_化%";
    -- 查询姓名是3个字的人
    SELECT * FROM student WHERE NAME LIKE '___';
    -- 查询姓名中包含德的人
    SELECT * FROM student WHERE NAME LIKE '%德%';
    ```

### 3. 排序查询

```mysql
select * from student order by 字段1 方式1, 字段2 方式2, ...
```

* 排序方式：
	* ASC：升序，默认的。
	* DESC：降序。
* 注意：
	* 如果有多个排序条件，则当前边的条件值一样时，才会判断第二条件。

### 4. 聚合函数

将一列数据作为一个整体，进行纵向的计算，得到某个字段的个数、最大/小值、和、均值等。

1. count：计算个数
	
	```mysql
	select count(字段名) from student -- 排除null值
	select count(ifnull(字段名,0)) from student -- 将null替换为0，计入count
	select count(*) from student -- 一般不使用
	```
	
	- 一般选择非空的列：主键
	
2. max：计算最大值

3. min：计算最小值

4. sum：计算和

5. avg：计算平均值
### 5. 分组查询:

根据某个字段（分组字段）分组，得到各组某个字段（查询字段）的特性。

语法：

```mysql
select 分组字段,聚合函数1(查询字段1),聚合函数2(查询字段2),... from 表名 group by 分组字段;
-- 对满足条件的条目分组
select 分组字段,聚合函数(查询字段) from 表名 where 条件 group by 分组字段;
-- 显示结果满足条件的分组
select 分组字段,聚合函数(查询字段) from 表名 group by 分组字段 having 条件;
```

注意：

1. 分组之后查询的字段：**分组字段、聚合函数**，其他字段的没有意义
2. where 和 having 的区别？
   - where 在分组**之前**进行限定，如果不满足条件，则不参与分组。having在分组**之后**进行限定，如果不满足结果，则不会被查询出来
   - where 后不可以跟聚合函数，having可以进行聚合函数的判断。

例如：

```mysql
-- 按照性别分组。分别查询男、女同学的平均分
SELECT sex , AVG(math) FROM student GROUP BY sex;
-- 按照性别分组。分别查询男、女同学的平均分,人数
SELECT sex , AVG(math),COUNT(id) FROM student GROUP BY sex;
-- 按照性别分组。分别查询男、女同学的平均分,人数。要求分数低于70分的人，不参与分组
SELECT sex , AVG(math),COUNT(id) FROM student WHERE math > 70 GROUP BY sex;
--  按照性别分组。分别查询男、女同学的平均分,人数。要求分组之后，人数要大于2个人
SELECT sex , AVG(math),COUNT(id) FROM student GROUP BY sex HAVING COUNT(id) > 2;
SELECT sex , AVG(math),COUNT(id) 人数 FROM student GROUP BY sex HAVING 人数 > 2;
```

### 6. 分页查询
1. 语法：

   ```mysql
   SELECT * FROM 表名 LIMIT 开始的索引,每页查询的条数;
   ```

2. 公式：

   - 开始的索引 = （当前的页码-1）* 每页显示的条数
   - 索引从0开始

3. limit 是MySQL特有的子句

4. 例如
    ```mysql
    -- 每页显示3条记录 
    SELECT * FROM student LIMIT 0,3; -- 第1页
    SELECT * FROM student LIMIT 3,3; -- 第2页
    SELECT * FROM student LIMIT 6,3; -- 第3页
    ```
## 约束

* 概念： 对表中的数据进行限定，保证数据的正确性、有效性和完整性。	
* 分类：
    1. 主键约束：primary key
    2. 非空约束：not null
    3. 唯一约束：unique
    4. 外键约束：foreign key

### 非空约束：not null

* 创建表时添加约束
    ```mysql
    CREATE TABLE stu(
        id INT,
        NAME VARCHAR(20) NOT NULL -- name为非空
    );
    ```
* 创建表完后，添加非空约束
    ```mysql
    ALTER TABLE 表 MODIFY 字段 VARCHAR(20) NOT NULL;
    ```

* 删除字段的非空约束 
    ```mysql
    ALTER TABLE 表 MODIFY 字段 VARCHAR(20);
    ```

### 唯一约束：unique

* 创建表时，添加唯一约束，即值不能重复
  
    ```mysql
    CREATE TABLE 表(
        id INT,
        字段 VARCHAR(20) UNIQUE -- 添加了唯一约束
    );
    ```
    
    * 注意mysql中，唯一约束限定的列的值可以有多个null
    
* 删除唯一约束
    ```mysql
    ALTER TABLE 表 DROP INDEX 字段;
    ```

* 在创建表后，添加唯一约束
    ```mysql
    ALTER TABLE 表 MODIFY 字段 VARCHAR(20) UNIQUE;
    ```

### 主键约束：primary key。
* 注意：
    - 含义：非空且唯一
    - 一张表只能有一个字段为主键
    - 主键就是表中记录的唯一标识

* 在创建表时，添加主键约束

    ```mysql
    create table stu(
        id int primary key,-- 给id添加主键约束
    name varchar(20)
    );
    ```

* 删除主键

    ```mysql
    ALTER TABLE stu DROP PRIMARY KEY;
    ```

* 创建完表后，添加主键

    ```mysql
    ALTER TABLE stu MODIFY id INT PRIMARY KEY;
    ```

* 自动增长：
    - 概念：如果某一列是数值类型的，使用 auto_increment 可以来完成值得自动增长
    - 在创建表时，添加主键约束，并且完成主键自增长

      ```mysql
      create table stu(
          id int primary key auto_increment,-- 给id添加主键约束
          name varchar(20)
      );
      ```

* 删除自动增长
    ```mysql
    ALTER TABLE stu MODIFY id INT;
    ```

* 添加自动增长
    ```mysql
    ALTER TABLE stu MODIFY id INT AUTO_INCREMENT;
    ```

### 外键约束
foreign key，让表于表产生关系，从而保证数据的正确性。

1. 在创建表时，可以添加外键
    ```mysql
    create table 表名(
        ....
        外键列
        constraint 外键名 foreign key (外键列) references 主表(主表列)
    );
    ```
    
2. 删除外键
   ```mysql
    ALTER TABLE 表名 DROP FOREIGN KEY 外键名;
   ```
   
3. 创建表之后，添加外键
   ```mysql
    ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列名称);
   ```
   
4. 级联操作
   
    ~~~mysql
    ```mysql
    ALTER TABLE 表名 ADD CONSTRAINT 外键名称 
    FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列名称) ON UPDATE CASCADE ON DELETE CASCADE  ;
    ```
    ~~~
    
    分类：
    
    - 级联更新：ON UPDATE CASCADE 
    - 级联删除：ON DELETE CASCADE 

## 数据库的设计

### 1. 多表之间的关系

**分类：**

1. 一对一(了解)：
   * 如：人和身份证
   * 分析：一个人只有一个身份证，一个身份证只能对应一个人
2. 一对多(多对一)：
   * 如：部门和员工
   * 分析：一个部门有多个员工，一个员工只能对应一个部门
3. 多对多：
   * 如：学生和课程
   * 分析：一个学生可以选择很多门课程，一个课程也可以被很多学生选择

**实现关系：**

1. 一对多(多对一)：
   * 如：部门和员工
   * 实现方式：在**多**的一方建立外键，指向**一**的一方的主键。
2. 多对多：
   * 如：学生和课程
   * 实现方式：多对多关系实现需要借助**第三张中间表**。中间表至少包含两个字段，这两个字段作为第三张表的外键，分别指向两张表的主键
3. 一对一(了解)：
   * 如：人和身份证
   * 实现方式：一对一关系实现，可以在任意一方添加**唯一**外键指向另一方的主键。

**案例**

|      表      | 字段                     | 关系 |
| :----------: | ------------------------ | ---- |
|   旅游分类   | 类别                     |      |
| 旅游线路分类 | 线路、名称、价格         |      |
|     用户     | 用户名、用户ID、用户密码 |      |



```mysql
案例
-- 创建旅游线路分类表 tab_category
-- cid 旅游线路分类主键，自动增长
-- cname 旅游线路分类名称非空，唯一，字符串 100
CREATE TABLE tab_category (
    cid INT PRIMARY KEY AUTO_INCREMENT,
    cname VARCHAR(100) NOT NULL UNIQUE
);

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

/*创建用户表 tab_user
uid 用户主键，自增长
username 用户名长度 100，唯一，非空
password 密码长度 30，非空
name 真实姓名长度 100
birthday 生日
sex 性别，定长字符串 1
telephone 手机号，字符串 11
email 邮箱，字符串长度 100
*/
CREATE TABLE tab_user (
    uid INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(100) UNIQUE NOT NULL,
    PASSWORD VARCHAR(30) NOT NULL,
    NAME VARCHAR(100),
    birthday DATE,
    sex CHAR(1) DEFAULT '男',
    telephone VARCHAR(11),
    email VARCHAR(100)
);

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

### 2. 数据库设计的范式

- 概念：设计数据库时，需要遵循的一些规范。
  - 设计关系数据库时，遵从不同的规范要求，设计出合理的关系型数据库，这些不同的规范要求被称为不同的范式，各种范式呈**递次规范**，越高的范式数据库冗余越小。
  - 目前关系数据库有六种范式：第一范式（1NF）、第二范式（2NF）、第三范式（3NF）、巴斯-科德范式（BCNF）、第四范式(4NF）和第五范式（5NF，又称完美范式）。
  - 要遵循后边的范式要求，必须先遵循前边的所有范式要求

* 分类：
    1. 第一范式（1NF）：每一列都是不可分割的原子数据项
    2. 第二范式（2NF）：在1NF的基础上，非码属性必须完全依赖于候选码（在1NF基础上消除非主属性对主码的**部分函数依赖**）
        * 几个概念：
            1. 函数依赖：A-->B,如果通过A属性(属性组)的值，可以确定唯一B属性的值。则称B依赖于A
                例如：学号-->姓名。  （学号，课程名称） --> 分数
            2. 完全函数依赖：A-->B， 如果A是一个属性组，则B属性值得确定需要依赖于A属性组中所有的属性值。
                例如：（学号，课程名称） --> 分数
            3. 部分函数依赖：A-->B， 如果A是一个属性组，则B属性值得确定只需要依赖于A属性组中某一些值即可。
                例如：（学号，课程名称） -- > 姓名
            4. 传递函数依赖：A-->B, B -- >C . 如果通过A属性(属性组)的值，可以确定唯一B属性的值，在通过B属性（属性组）的值可以确定唯一C属性的值，则称 C 传递函数依赖于A
                例如：学号-->系名，系名-->系主任
            5. 码：如果在一张表中，一个属性或属性组，被其他所有属性所**完全依赖**，则称这个属性(属性组)为该表的码
                例如：该表中码为：（学号，课程名称）
                * 主属性：码属性组中的所有属性
                * 非主属性：除过码属性组的属性
        
    3. 第三范式（3NF）：在2NF基础上，任何非主属性不依赖于其它非主属性（在2NF基础上**消除传递依赖**）

### 3. 数据库的备份和还原

1. 命令行：
    * 备份： `mysqldump -u用户名 -p密码 数据库名称 > 保存的文件路径名`
    * 还原：
        1. 登录数据库：`mysql -u用户名 -p密码`
        2. 创建数据库：`create database 数据库名`
        3. 使用数据库：`use 数据库名`
        4. 执行文件：`source 保存的文件路径名`
2. 图形化工具SQLyog：
    - 备份：右键数据库名称，选择”备份/导出“，选择”备份到数据库“，选择路径，导出
    - 还原：右键“root@localhost”，选择执行SQL脚本，选择路径，执行

## 多表查询

### 1. 什么是多表查询

所谓多表查询就是联合查询多张表的数据

- 笛卡儿积现象

  ```mysql
  select * from emp,dept; -- 得到的条目数是两个表条目数的乘积，包含无用数据
  ```

- 消除无用数据：设置过滤条件

  ```mysql
  select * from emp,dept where emp.`dept_id` = dept.`id`;
  ```

### 2. 多表查询的分类

<img src="https://raw.githubusercontent.com/hihanying/FigureBed/master/img/20191104235147.png" style="zoom:50%;" />

1. 内连接

   用左边表的记录去匹配右边表的记录，如果符合条件的则显示。

   - 隐式内连接：使用where条件消除无用数据

     ```mysql
     SELECT 字段名 FROM 左表, 右表 WHERE 条件;
     -- 简单形式
     SELECT * FROM emp,dept WHERE emp.`dept_id` = dept.`id`;
     -- 标准形式
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

   -  显示内连接：使用 `INNER JOIN ... ON` 语句, 可以省略`INNER `

     ```mysql
     SELECT 字段名 FROM 左表 [INNER] JOIN 右表 ON 条件;
     ```

     



## 事务

### 1. 事务的概念

### 2. 事务的四大特征

- 原子性：
- 持久性：
- 隔离性：
- 一致性：事务操作前后，数据总量不变

### 3. 事务的隔离级别

* 概念：多个事物之间是隔离的，相互的独立的



- 存在问题

  1. 脏读：一个事务读取到另一个事务中没有提交的数据
  2. 虚读/不可重复读：在同一个事务中，两次读取的数据不一样
  3. 幻读：

- 隔离级别

  - read uncommitted：读未提交
    - 产生问题：
  - read committed：读已提交（Oracle默认）
    - 产生问题：
  - repeatable read：可重复读（MySQL默认）
    - 产生问题：
  - serializable：串行化
    - 可以解决所有问题

  > **注意：**
  >
  > - 隔离级别从小到大安全性越来越高，但是效率越来越低
  > - 数据库查询隔离级别：
  > - 数据库设置隔离级别：
  > - 重新打开才能生效

  





## DCL：管理用户/控制权限

DBA：数据库管理员

### 管理用户

1. 添加用户

   ```mysql
   
   ```

2. 删除用户

   ```mysql
   DROP USER '用户名'@'主机名'; 
   ```

3. 修改密码 

   控制台修改管理员密码（需要管理员权限）：

   ```shell
   mysqladmin -uroot -p password 新密码
   ```

   > 注意：需要在未登陆 MySQL 的情况下操作，新密码不需要加上引号。

   修改普通用户密码：

   ```mysql
   set password for '用户名'@'主机名' = password('新密码'); -- 注意：需要在登陆 MySQL 的情况下操作，新密码要加单引号。
   ```

   

4. 查询用户

   ```mysql
   
   ```

     

### 权限管理

1. 查询权限

   ```mysql
   
   ```

2. 授予权限

   ```mysql
   
   ```

3. 撤回权限

   ```mysql
   
   ```









