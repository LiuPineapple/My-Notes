# SQL教程-笔记

## 1 关系型数据库概述

一、任何SQL语句最后都要加;表示这一逻辑行的结束

二、SQL语句不区分大小写

三、数据类型

- DECIMAL(m,n)并不代表真的要到这种精度，只是设置最大精度
- ENUM('A','B','C',...) 枚举类，数据只能从中选取

四、 DDL操作，数据库名前面要加DATABASE，表名前面要加TABLE，而DML和DQL不需要加，这一点也很容易理解，DDL中的删除都是DROP

五、SQL中的注释

https://blog.csdn.net/TaoGodEmperor/article/details/82746603

## 2 安装MySQL

一、mysql和mysqld命令的区别

两个都是在环境变量里的.exe程序，前者是客户端后者是服务端。不过因为现在安装的时候都把mysql服务设置为本机网络服务了，所以不用在cmd里运行mysqld了，可以直接：

1. wind+r找services.msc找到服务并打开或关闭
2. 进入cmd命令行然后`net start mysql80`或者`net stop mysql80`最后面的mysql80是本机mysql服务的名字

见：https://zhidao.baidu.com/question/279891181.html

## 4 查询数据

#### 4.2 条件查询

注意在SQL中相等只有一个等号，不等为<>

#### 4.6 聚合查询

分组聚合查询的列中，如果是SUM，COUNT,AVG只能放入聚合函数和，分组后，每个组中的值唯一的列，而不是只能放入分组的列。

如果是MAX,MIN,也可以放入非唯一的列，这时候会选出最大值或者最小值所对应的那一行来

## 5 修改数据

一、INSERT

INSERT INTO 表名

后面可以不加括号，表示全列插入，这时候VALUES后面的括号中值的顺序必须跟表中顺序一致，如果对于自增主键，不想输入值，可以使用占位符：0,NULL,DEFAULT （只能用于自增主键）

eg:INSERT INTO students VALUES(DEFAULT,...,...),(),...

## 6 MySQL

#### 6.1 管理MySQL

一、退出MySQL可以用quit或者exit。注意这两个命令仅仅断开了客户端和服务器的连接，MySQL服务器仍然继续运行

二、常用命令补充：

1.  查看当前版本：SELECT VERSION();

2. 查看当前时间：SELECT NOW();

3. 查看当前正在使用的数据库：SELECT DATABASE(); 

4. 修改输入提示符：PROMPT python>  

   python> 只是一个例子，可以改成别的

   /\U/\D 前者以用户名为提示符，后者以当前日期为提示符

5. 创建指定字符集的数据库：CREATE DATABASE test CHARSET=UTF8

   最后的test,UTF8是数据库名和字符集名

6. 显示创建数据库或者表的命令：

   SHOW CREATE DATABASE test

   SHOW CREATE TABLE students

   最后的test,students是数据库名和表名

7. 创建数据表：

   CREATE TABLE students(字段名 字段类型 字段约束,...,...)             students为数据表名

   eg: CREATE TABLE students(id INT UNSIGNED PRIMARY KEY AUTO_INCREMENT,

   ​                                                  name VARCHAR(10) NOT NULL,

   ​                                                  age TINYINT UNSIGNED DEFAULT 0,

   ​                                                  high DECIMAL(5,3) DEFAULT 0.0,

   ​                                                  gender ENUM('男','女','中性','保密')  DEFAULT '保密'

   ​                                                   );

   在id字段后面加上PRIMARY KEY是将该字段设置为主键，或者在表的最后一行加上PRIMARY KEY(id)

   如果想使用联合主键也可以PRIMARY KEY(id,name,...)

8. 如果不想改字段名只想改其他，可以用ALTER MODIGY

   eg: ALTER TABLE students MODIFY COLUMN birthday DATE DEFAULT '2011-11-11'

#### 6.2 实用SQL语句

1. REPLACE INTO,ON DUPLICATE KEY等的用法见如下链接：
   - https://www.cnblogs.com/flyingeagle/articles/9825949.html
   - https://www.cnblogs.com/sweet521/p/5730804.html

2. 在插入数据时，无论是哪个语句，所检查的都是PRIMARY或UNIQUE INDEX，也就是这两个中任意一个重复就不能直接插入

## 补充

1. sql窗口函数：

   - https://www.cnblogs.com/DataArt/p/9961676.html

   - 大数据分析之hive学习\第5节 HQL窗口函数

2. Mysql随机数的生成：
   - https://www.cnblogs.com/zejin2008/p/4710364.html
   - https://www.cnblogs.com/fps2tao/p/9041204.html
   - 

3. 变量

   https://www.cnblogs.com/Brambling/p/9259375.html

   https://blog.csdn.net/zhou520yue520/article/details/81146469

   https://www.cnblogs.com/aixinyiji/p/11033297.html

# Hive SQL的使用

1. 高级聚合

   - https://cwiki.apache.org/confluence/display/Hive/Enhanced+Aggregation%2C+Cube%2C+Grouping+and+Rollup
   - 对于grouping(column)来说，如果对应列在这一行被聚合了（也就是为NULL）那么值就为1，否则为0。这里需要注意，一列值为NULL并不代表这一列被聚合了，也可能是这一列本身值为NULL，这里就体现出grouping和grouping__ID的作用了。
   - 对于grouping__ID来说，把每一列的grouping结果拼在一起为一个二进制数然后转换成十进制
   - grouping sets要跟group by一起使用，这时候group by用于声明字段，结果是去重并默认排序的相当于union。结果没有对所有项的聚合除非你加一个()
   - rollup和cube都是默认有对所有列聚合的一行的
   - rollup和cube两种用法，既可以放在group by后面，with rollup/cube，也可以group by colume,...,cube/rollup(column,...,column) 。如果第二种方法的cube和rollup前面没有列，那么就等同于第一种方法，如果前面有列，就相当于先按照前面的列聚合，在每一个分组中再按照cube或者rollup聚合

2. 窗口函数

   大数据分析之hive学习\第5节 HQL窗口函数
   
3. 时间处理函数，需要记住的有

   - DATE_ADD(),DATE_SUB(),DATEDIFF()

      https://blog.csdn.net/qq_35958094/article/details/80460644

     Hive中只有Date和timestamp两种时间类型，DATEDIFF()使用与Mysql相同，都是默认不考虑时钟值，DATE_ADD(),DATE_SUB()第二个参数没有INTERVAL关键字，只能跟天数。这里需要注意，Hive和Mysql中timestamp类型都是类似于Datetime格式的，而不是秒数。

   - unix_timestamp() 在Hive中，这个函数有两个参数，但是在Mysql中，这个函数只有一个参数

   - from_unixtime() 在Hive中和在Mysql中，这个函数有两个参数，第二个参数有默认值可以不写

     https://blog.csdn.net/xienan_ds_zj/article/details/104482728

4. 字符串处理函数，需要记住的有

   - concat(),concat_ws(),group_concat()

     https://www.cnblogs.com/wqbin/p/10266783.html

     这三个函数和Mysql中的用法一样

   - split()，产生一个array

   - substr

5. 

6. 

7. 

   

   