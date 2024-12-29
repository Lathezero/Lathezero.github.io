---

title: MySQL 基础教程

date: '2024-12-23'

categories: 
  - [后端开发, 数据库, MySQL]
  - [教程指南, 入门教程]

description: MySQL 是最流行的关系型数据库之一，本文将介绍其基本概念和常用命令...

tags: ['MySQL', '数据库', '后端开发']

keywords: MySQL, 数据库, 后端开发

---

# DDL数据库定义语言

## 数据库

#### 操作语法

1. 查询所有数据库

   ```sql
   show databases;
   ```

2. 查询当前数据库

   ```sql
   select database();
   ```

3. 使用/切换数据库

   ```sql
   use 数据库名;
   ```

4. 创建数据库

   ```sql
   create database [if not exists] 数据库名 [default charset utf8mb4];
   ```

   MySQL8.0 版本及以上字符集默认为 utf8mb4

5. 删除数据库

   ```sql
   drop database [if existe] 数据库名;
   ```

#### 以上的 database 均可替换成 ```plaintext schema``` 如: ```sql creat schema database db01;```

注：在 show 语句中把 databases 替换成 schemas 需要加 s 作为复数形式

## 表结构

#### 创建

```sql
creat table tablename (
	字段1 字段类型 [约束] [comment 字段1注释],
	字段2 字段类型 [约束] [comment 字段2注释]
)[comment 表注释];
```

约束：非空约束```plaintext not null```

唯一约束```plaintext unique```

主键约束```plaintext primary key```

默认约束```plaintext default```

外健约束```plaintext foreign key```

#### 查询、修改、删除

1. 查询当前数据库所有表

   ```sql
   show tables
   ```

2. 查看表结构

   ```sql
   show 表名;
   ```

3. 查询建表语句

   ```sql
   show create table 表名;
   ```

4. 添加字段

   ```sql
   alter table 表名 add 字段名 类型 [约束] [注释];
   ```

5. 修改字段类型

   ```sql
   alter table 表名 modify 字段名 类型 [注释];
   ```

6. 修改字段名

   ```sql
   alter table 表名 change 字段名 新字段名 类型 [注释];
   ```

7. 删除字段

   ```sql
   alter table 表名 drop column 字段名;
   ```

8. 修改表名

   ```sql
   alter table 表名 rename to 新表名;
   ```

9. 删除表

   ```sql
   drop table 表名;
   ```

# DML 数据库操作语言 增删改

### insert

1. 指定字段添加数据 *

   ```sql
   insert into 表名 (字段1，字段2...) values (值1，值2);
   ```

2. 全部字段添加数据

   ```sql
   insert into 表名 values (值1，值2);
   ```

3. 指定字段批量添加数据 *

   ```sql
   insert into 表名 (字段1，字段2...) values (值1，值2), (值1，值2);
   ```

4. 全部字段批量添加数据

   ```sql
   insert into 表名 values (值1，值2), (值1，值2);
   ```

### updata

1. 修改单个数据

   ```sql
   updata 表名 set 字段 = '值', 字段 = '值' [where id = '值'];
   ```

2. 修改全部数据

   ```sql
   updata 表名 set 字段 = '值', 字段 = '值';
   ```

### dalete

1. 删除单条数据

   ```sql
   delete from 表名 where id = '值';
   ```

2. 删除全部数据

   ```sql
   delete from 表名;
   ```

# DQl

#### 基本查询

1. 查询多个字段

   ```sql
   select 字段1，字段2，字段3 from 表名;
   ```

2. 查询所有字段

   ```sql
   select * from 表名;
   ```

3. 为查询字段设置别名，as 关键字可以省略

   ```sql
   select 字段1 [as 别名1]，字段2[as 别名2] from 表名;
   ```

4. 去除重复记录

   ```sql
   select distinct 字段列表 from 表名;
   ```

#### 条件查询

```sql
select 字段列表 from 表名 where 条件列表;
```

#### 聚合函数

1. count

   ```sql
   select count(id) from 表名;
   ```

   ```sql
   select count(*) from 表名;
   ```

   ```sql
   select count(1) from 表名;
   ```

2. 其他

   ```sql
   select max(字段) from 表名;
   ```

   ```sql
   select min(字段) from 表名;
   ```

   ```sql
   select avg(字段) from 表名;
   ```

   ```sql
   select sum(字段) from 表名;
   ```

```sql
select
	字段列表
from
	表名列表
where
	条件列表
group by
	分组字段列表
having
	分组后条件列表
order by
	排序字段列表
limit
	分页参数
```

#### 分组查询

```sql
select 字段列表 from 表名 [where 条件列表] group by 分组字段名 [having 分组后过滤条件];
```

```sql
select job, count(*) from emp where data <= '2015-01-01' group by job havin count(*) >= 2;
```

where 和 havin 的区别：

1. 执行时机不同 (where -> group by -> having)
2. 判断条件不同 (having 后可以用聚合函数，where 不可以)

#### 排序查询

```sql
select 字段列表 from 表名 [where 条件列表] [group by 分组字段名 having 分组后过滤条件] order by 排序字段 排序方式
```

```sql
select * from emp order by entry_date asc, update_time desc;
```

升序：asc （默认） 降序：desc

#### 分页查询

```sql
selece 字段列表 from 表名 [where 条件] [group by 分组字段 having 过滤条件] [order by 排序字段] limit 起始索引, 查询记录数;
```

起始索引为 0 时可以省略

