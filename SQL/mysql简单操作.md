#### mysql简单操作

##### 1. 修改提示符

```mysql
prompt 提示符   eg： \h

参数： 
\D 完整的日期
\d 当前数据库
\h 服务器名称
\u 当前的用户
```

##### 2. mysql常用命令

- 显示当前服务器版本

  ```mysql
  SELECT VERSION();
  ```

  

- 显示当前的日期时间

  ```mysql
  SELECT NOW();
  ```

  

- 显示当前的用户

  ```mysql
  SELECT USER();
  ```

##### 3. mysql语句的规范

- 关键字与函数名称全部大写
- 数据库名称、表名称。字段名称全部小写
- SQL语句必须以分号结尾

##### 4.数据库操作

> {} 代表必选项
>
> | 代表选择
>
> [] 代表可选项

- **创建数据库**

  ```mysql
  CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name [DEEAULT] CHARACTER SET [=] charset_name 
  
  // 设定编码gbk;
  CREATE DATABASE IF NOT EXISTS t1 CHAARACTER SET gbk;
  
  ```

- 查看警告信息

  ```mysql
  SHOW WARNING;
  ```

- 查看创建数据库时候的命令

  ```mysql
  SHOW CREATE DATABASE t1;
  ```

- 查询数据库

  ```mysql
  SHOW {DATABASES | SCHEMAS}
  {LIKE 'pattern' | WHERE expr}
  ```

- **修改数据库**

  ```mysql
  ALTER {DATABASE | SCHEMA} [db_name] [DEEAULT] CHARACTER SET [=] charset_name
  ```

- **删除数据库**

  ```mysql
  DROP {DATABASE | SCHEMA} [IF EXISTS] db_name
  ```

- 登录数据库

  ```mysql
  mysql -u root -p  ----cjjxw0320
  ```

- 使用数据库

  ```mysql
  USE 数据库名字
  ```

- 创建数据表

  ```mysql
  CREATE TABLE [IF NOT EXISTS] table_name(
  	column_name data_type,
  )
  ```

- **查看数据表列表**

  ```mysql
  SHOW TABLES [FROM db_name] [LIKE 'pattern' | WHERE expr]
  ```

- 选中当前数据库

  ```mysql
  SELECT DATABASE();
  ```

- 查看数据表结构

  ```mysql
  SHOW COLUMNS FROM tbl_name;
  DESC tbl_name;
  ```

- 插入记录

  ```mysql
  INSERT [INTO] tbl_name [(col_name,....)] {VALUES|VALUE} ({expr|DEFAULT},...),(...)
  ```

- 记录查找

  ```mysql
  SELECT select_expr [,select_expr]
  [
      FROM  table_references
      [WHERE where_condition]
      [GROUP BY {col_name|position} [ASC |DESC],..]
      [HAVING where_condition]
      [ORDER BY {col_name | expr | position} [ASC |DESC],...]
      [LIMIT {[oddset,] row_count | row_count OFFSET offset}]
  ]
  ```

- 插入记录

  ```mysql
  1.INSERT [INTO] tbl_name [(col_name,....)] {VALUES|VALUE} ({expr|DEFAULT},...),(...)
  
  2.INSERT [INTO] tbl_name SET col_name = {expr | DEFAULT},.....
  
  说明：与第一种方式的区别在于，此方法可以使用子查询(SubQuery)
  
  3.INSERT [INTO] tbl_name [(COL_NAME,...)] SELECT ...
  
  说明：此方法可以将查询结果插入指定的数据表
  ```

- 更新记录（单表更新）

  ```mysql
  UPDATE [LOW_PRIORITY] [IGNORE] table_refetence SET col_name1 = {expr1|DEFAULE}, col_name2 = {expr2 | DEFAULT}]... [WHERE where_condition]
  
  eg: UPDATE users set age = age + 5 [where id % 2 =0];
  ```

- 多表更新

  ```
  UPDATE table_references 
  SET col_name1 = {expr1 |DEFAULT}
  [.col_name2 = {expr2|DEFAULT}] ...
  [WHERE where_condition]
  ```

- 创建数据表同事将查询结果写入到数据表

  ```
  CREATE TABLE [IF NOT EXITS] tbl_name
  [(create_definition,...)]
  select_statement
  ```

  

- 单表删除

  ```mysql
  DELETE FROM table_name [WHERE where_condition]
  删除某个表的某一条数据
  ```

- 修改数据表

  ```mysql
  添加单列
  ALTER TABLE table_name ADD [COLUMN] col_name column_definition [FIRST|AFTER col_name] 
  
  添加多列
  ALTER TABLE tbl_name ADD [COLUMN] (col_name column_definition,...)
  
  删除列
  ALTER TABLE tnl_name DROP [COLUMN] col_name
  ```

- 添加主键约束

  ```mysql
  ALTER TABLE tbl_name ADD [CONSTRAINT [SYMBOL]] PRIMARY KEY [index_type] (index_col_name,...)
  ```

- 删除主键约束

  ```mysql
  ALTER TABLE users2 DROP PRIMARY KEY;
  ```

- 修改列定义

  ```mysql
  ALTER TABLE tbl_name MODIFY [COLUMN] col_name column_definition [first|after col_name]
  ```

- 修改列名称

  ```mysql
  ALTER TABLE TBL_NAME CHANGE [COLUMN] old_col_name new_col_name column_definition [FIRST|AFTER col_name]
  ```

- 数据表更名

  ```mysql
  方法1 
  ALTER TABLE tbl_name RENAME [TO|AS] new_tbl_name
  
  方法2
  RENAME TABLE tbl_name TO new_tbl_name [,tbl_name2 TO new_tbl_name2] ...
  ```

- 连接

  ```mysql
  table_reference
  {[INNER|CROSS] JOIN | {LEFT|RIGHT} [OUTER] JOIN}
  table_reference
  ON conditional_expr
  
  备注：
  table_refernce
  tbl_name [[AS] alias| table_subquery [AS] alias
  数据表可以使用tbl_name AS alias_name 或者 tbl_name alias_name 赋予别名
  table_subquery可以作为子查询使用在FROM子句中，
  这样的子查询必须为其赋予别名
  ```

- 连接类型

  > INNER JOIN ,内连接
  >
  > 在MySqL中，JOIN,CROSS JOIN 和INNER JOIN是等价的
  >
  > LEFT [OUTER] JOIN ,左外连接
  >
  > RIGHT [OUTER] JOIN ，右外连接

  - 连接条件

    > 使用ON关键字来设定连接条件，也可以使用WHERE 来代替。
    >
    > 通常使用ON关键字来设定连接条件
    >
    > 使用WHERE关键字进行结果集记录的过滤

  - eg:

    ```mysql
    内连接：
    
    SELECT goods_id,goods_name,cate_name FROM tdb_goods INNER JOIN tdb_goods_cates
    ON tdb_goods.cate_id = tdb_goods_cates.cate_id;
    
    左外连接
    
    SELECT goods_id,goods_name,cate_name FROM tdb_goods LEFT JOIN tdb_goods_cates
    ON tdb_goods.cate_id = tdb_goods_cates.cate_id;
    
    右外连接
    
    SELECT goods_id,goods_name,cate_name FROM tdb_goods RIGHT JOIN tdb_goods_cates
    ON tdb_goods.cate_id = tdb_goods_cates.cate_id;
    
    ```

    

  



- 分组

  ```mysql
  SELECT age FROM users  GROUP BY HAVING age > 35;
  ```

- 对查询结果进行排序

  ```mysql
  SELECT * FEOM users ORDER BY id DESC;
  ```

- 限制查询返回的数量

  ```mysql
  [LIMIT {[offset,] row_count|row_count OFFSET offset}]
  
  eg, SELECT * FROM users LIMIT 2,2
  ```

- 更改结束符号

  ```mysql
  DELIMITER //    改成//
  DELIMITER ;    改成;
  ```

- 查看数据表的创建命令

  ```mysql
  SHOW CREATE TABLE tb_name;
  ```

