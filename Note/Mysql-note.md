### MySQL常用引擎介绍
- MyISAM存储引擎：不支持事务、也不支持外键，优势是访问速度快，对事务完整性没有 要求或者以select，insert为主的应用基本上可以用这个引擎来创建表
- InnoDB存储引擎：该存储引擎提供了具有提交、回滚和崩溃恢复能力的事务安全。但是对比MyISAM引擎，写的处理效率会差一些，并且会占用更多的磁盘空间以保留数据和索引。 InnoDB存储引擎的特点：支持自动增长列，支持外键约束
- MEMORY存储引擎：Memory存储引擎使用存在于内存中的内容来创建表。每个memory表只实际对应一个磁盘文件，格式是.frm。memory类型的表访问非常的快，因为它的数据是放在内存中的，并且默认使用HASH索引，但是一旦服务关闭，表中的数据就会丢失掉。 MEMORY存储引擎的表可以选择使用BTREE索引或者HASH索引，两种不同类型的索引有其不同的使用范围
- MERGE存储引擎：是一组MyISAM表的组合，这些MyISAM表必须结构完全相同，merge表本身并没有数据，对merge类型的表可以进行查询，更新，删除操作，这些操作实际上是对内部的MyISAM表进行的。

### 字符串类型
- 数值类型  INT,FLOAT,DOUBLE
- 字符串类型  CHAR,VARCHAR,TEXT
- 日期和时间类型  DATA,TIME,YEAR,DATETIME

### SQL语言共分为四大类
- 数据定义语言DDL
- 数据操纵语言DML
- 数据查询语言DQL
- 数据控制语言DCL

#### DDL数据定义语言
- 创建数据库  
CREATE DATABASE 数据库名 CHARACTER SET UTF8
- 修改数据库  
ALTER DATABASE 数据库名 CHARACTER SET GBK
- 查看数据库  
SHOW DATABASES
- 添加一列  
ALTER TABLE 表名 ADD 列名 数据类型
- 查看表的字段信息  
DESC 表名
- 修改表的字段类型  
ALTER TABLE 表名 MODIFY 字段名 数据类型  
- 删除一列  
ALTER TABLE 表名 DROP 字段名
- 修改表名  
RENAME TABLE 原表名 TO 新表名
- 查看表的创建细节  
SHOW CREATE TABLE 表名
- 修改表的字符集  
ALTER TABLE 表名 DEFAULT CHARACTER SET 字符集
- 修改表的列名
ALTER TABLE 表名 CHANGE 列名 新列名
- 删除表  
DROP TABLE 表名


#### DML数据操纵语言-对表中数据进行增删改
- 插入数据  
INSERT INTO 表名（列名1，列名2...) VALUES(列值1，列值2...)
- 更新数据  
UPDATE 表名 SET 列名1=列值1，...  
WHERE 列名=值  
- 删除数据  
DELETE FROM 表名  
[WHERE 条件]；  
TRUNCATE TABLE 表名；  
两者的区别：DELETE 删除表中的数据，表结构还在；删除后的数据还可以找回  
          TRUNCATE 将表DROP掉，创建一个新的表，数据无法找回，但删除速度更快


#### DQL数据查询语言
- FROM表，就是对表进行遍历
- SELECT后是运算表达式，应用到每一行，返回一个精准唯一到值；其运算表达式只有三种情况:常量，分组字段，聚合函数
- 查询表全部数据  
SELECT * FROM 表名  
- 逐行运算模型
- 分组运算模型
- 将查询记录转换为大写  
SELECT UPPER(列名) FROM 表名
- 分组查询  
   - GROUP BY + GROUP_CONCAT(字段名)  
   - GROUP BY + 聚合函数  
   - GROUP BY + HAVING
- LIMIT  
SELECT * FROM 表名 LIMIT 0,3——从第0行开始，查3行
- 数据的完整性  
   - 实体完整性——表中每一行记录代表一个实体  
       - 约束类型
          - 主键约束(PRIMARY KEY)  
          - 唯一约束(UNIQUE)  
          - 自动增长列(AUTO_INCREMENT)
   - 域完整性  
       - 域完整性约束
          - 数据类型:数值类型、日期类型、字符串类型  
          - 非空约束(NOT NULL)  
          - 默认值约束(DEFAULT)
- 参照完整性——设置外键
- 表关系:一对一，一对多，多对多;拆分表的目的是避免出现大量冗余数据
- 多表查询   
   - 合并结果集——UNION(默认去重) / UNION ALL(显示所有)  
   - 连接查询——笛卡尔积
   - 内连接   
      - 等值连接 
         - SELECT * FROM stu st   
         INNER JOIN score sc ON st.id = sc.id  
         INNER JOIN course co ON sc.cid = co.cid
         - ON 后面只写主外键
         - 还有其他条件，直接在后面写WHERE
      - 多表连接
         - 99连接法
         - 内联查询 使用INNER JOIN ON 的方式
      - 非等值连接
   - 外连接
      - 左外连接 LEFT JOIN
      - 右外连接 RIGHT JOIN  
   - 子查询
      - where后
      - from后
   - 自然连接 NATURAL JOIN

#### 常用函数
- 函数分类
   - 字符串函数
      - CONCAT() —— 将传入字符连接成一个字符串，任何字符与NULL连接的结果均为NULL
      - INSERT('str',x,y,'instr') —— 将字符串str从x位置开始，y个字符长度替换为instr字符串
      - LOWER()和UPPER() —— LOWER()将字符串全部转为小写；UPPER()将字符串全部转为大写。
      - LEFT(str,x) 与 RIGHT(str,x) —— 从字符串左边或右边取x位字符
      - LPAD('str',x,pad) 与 RPAD() —— 从字符串左边或右边用pad字符串进行填充，直到字符串位数达到要求的x位
      - LTRIM(str) 与 RTRIM(str) —— 去掉字符串中最左侧或最右侧的空格
      - TRIM(str) —— 去掉字符串左右两侧的空格
      - REPEAT(str,x) —— 返回str重复x次的结果
      - REPLACE(str,a,b) —— 将字符串中a字符串替换成b字符串
      - SUBSTRING(str,x,y) —— 返回字符串str从第x位起y个字节长度的字符串
   - 数值函数
      - ABS(x) —— x的绝对值
      - CEIL(x) —— 向上取整
      - FLOOR(x) —— 向下取整
      - MOD(x,y) —— x/y的模,即余数
      - RAND() —— 0-1取随机值
   - 日期和时间函数
      - CURDATE() —— 返回当前日期，只包含年月日
      - CURTIME() —— 返回当前时间，只包含时分秒
      - NOW() —— 返回当前日期与时间，年月日时分秒
      - UNIX_TIMESTAMP() —— 返回当前日期的时间戳
      - FROM_UNIXTIME(unixtime) —— 将时间戳转换成日期
      - WEEK(DATE) —— 返回当前是一年中的第几周
      - YEAR(DATE) —— 返回当前是哪一年
      - HOUR(TIME) —— 返回当前是几点
      - MINUTE(TIME) —— 返回当前是几分
      - DATE_FORMAT(date,format) —— 按字符串格式化日期date值
      - DATE_ADD(date,INTERVAL expr unit) —— 计算日期间隔
      - DATEDIFF(DATE1,DATE2) —— 计算两个日期相差天数
   - 流程函数
      - IF(value,T,F) —— 如果value是真，返回T，否则返回F
      - IFNULL(value1,value2) —— 如果value1不为空，则返回value1否则返回value2
      - CASE **_case_value_** WHEN _**when_value**_ THEN _**statement_list**_ ELSE _**statement_list**_  END CASE;
   - 其他常用函数
      - DATABASE() —— 返回当前数据库名
      - VERSION() —— 返回当前数据库版本
      - USER() —— 返回当前登录用户名
      - PASSWORD(str) —— 对str进行加密
      - MD5(str) —— 返回str对MD5值

#### 事务
- 事务是不可分割的操纵，每一条sql语句都是一个事务，事务只对DML语句有效
- 事务的ACID
   - 原子性(Atomicity) —— 要么都做，要么都不做。
   - 一致性(Consistency) —— 与原子性相关，多个事物的一致，数据的一致。
   - 隔离性(Isolation) —— 一个事物的执行不能被其他事物干扰。
   - 持久性(Durability) —— 一旦提交，对数据库中的数据的改变是永久性的。
- 事务的使用
   - 开始事务 START TRANSACTION
   - 提交事务 COMMIT
   - 回滚事务 ROLLBACK
- 事务并发问题与隔离级别  
   事务隔离级别 | 脏数据 | 不可重复读 | 幻读  
   -|-|-|-  
   读未提交(Read uncommitted) | 是 | 是 | 是 |  
   不可重复读(Read committed) | 否 | 是 | 是 |  
   可重复读(Repeatable read) | 否 | 否 | 是 |  
   串行化(Serializable) | 否 | 否 | 否 |  

#### 权限
- 授予权限  
GRANT <权限>  
ON <对象类型><对象名>   
TO <用户>  
[WITH GRANT OPTION] —— 可以将获得的权限授予其他用户
- 授予权限  
GRANT <权限>  
ON <对象类型><对象名>   
TO <用户>  
[WITH GRANT OPTION] —— 可以将获得的权限授予其他用户

#### 索引
- 索引优劣势
   - 优势
   - 劣势
- 索引的分类
   - 单值索引
   - 唯一索引
   - 复合索引
   - 全文索引