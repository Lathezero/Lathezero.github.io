---
title: MyBatis
date: 2025-01-06 21:38:56
categories:
  - [后端开发, Java]
  - [开发工具, 框架]
tags: ['MyBatis', 'Java', '后端开发', '框架']
description: MyBatis笔记
keywords: MyBatis, Java, 后端开发, 框架
---

## 准备工作

### 1. 创建Springboot模块，引入Mybatis相关依赖

	1. Lombok
	2. MyBatis Framework
	3. MySQL Driver

### 2. 准备数据库表和实体类

### 3. 配置Mybatis

```
spring.datasource.url=jdbc:mysql://localhost:3306/db01
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.username=root
spring.datasource.password=1234
```

## 编写Mybatis程序：编写Mybatis的持久层接口，定义SQL(注解/XML)

### 1. 定义Mapper接口(@Mapper)，编写SQL

```
@Mapper // 应用程序在运行时，会自动的为该接口创建一个实现类对象(代理对象)，并且会自动将该实现类对象存入IOC容器
public interface UserMapper {

    @Select("select * from user")
    public List<User> findAll();
}
```

### 2. 编写测试类

```
// springboot单元测试的注解--当前测试类中的测试方法运行时，会启动springboot项目
@SpringBootTest //就是说会加载IOC容器中的对象
class SpringbootMybatisQuickstartApplicationTests {
    @Autowired
    private UserMapper userMapper;

    @Test
    public void testFindAll() {
        List<User> users = userMapper.findAll();
        users.forEach(System.out::println);
    }

}
```

@SpringBootTest: 会在单元测试运行时，加载springBoot的环境

注意: 测试类所在包需要与引导类包名相同(或放在引导类所在包的子包下)

## 辅助配置

### SQL语句提示

IDEA中连接数据库即可

### 配置MyBatis的日志输出

在配置文件中写入：

```
mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
```

![mybatislog](images/MyBatis/mybatislog.png)

![mybatislog-stdoutimpl](images/MyBatis/mybatislog-stdoutimpl.png)

![log](images/MyBatis/log.png)

## 数据库连接池

1. 数据库连接池是个容器，负责分配、管理数据库连接(Connection)

2. 它允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个。

3. 释放空闲时间超过最大空闲时间的连接，来避免因为没有释放连接而引起的数据库连接遗漏

标准接口:DataSource

1. 官方(sun)提供的数据库连接池接口，由第三方组织实现此接口。
   2. 功能:获取连接`Connection getConnection()throws SQLException;`

常见产品：

​	C3P0、DBCP、Druid、Hikari(springboot默认)

​	Druid(德鲁伊)
​	Druid连接池是阿里巴巴开源的数据库连接池项目功能强大，性能优秀，是Java语言最好的数据库连接池之一

### 切换数据库连接池

1. springboot配置文件中配置德鲁伊依赖

   ```
   <dependency>
       <groupId>com.alibaba</groupId>
       <artifactId>druid-spring-boot-starter</artifactId>
       <version>1.2.19</version>
   </dependency>
   ```

2. mybatis配置文件中添加德鲁伊依赖

   ```
   spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
   ```

## 增删改查

### 删除操作

Mapper

```
@Mapper // 应用程序在运行时，会自动的为该接口创建一个实现类对象(代理对象)，并且会自动将该实现类对象存入IOC容器
public interface UserMapper {

    @Select("select id from user")
    public List<User> findAll();

    @Delete("delete from user where id=#{id} ")
    // public void delete(Integer id);
    public Integer deleteById(Integer id);
}
```

调用

```
@SpringBootTest // springboot单元测试的注解--当前测试类中的测试方法运行时，会启动springboot项目
class SpringbootMybatisQuickstartApplicationTests {
    @Autowired
    private UserMapper userMapper;

    @Test
    public void testFindAll() {
        List<User> users = userMapper.findAll();
        users.forEach(System.out::println);
    }

    @Test
    public void testDelete() {
        Integer i = userMapper.deleteById(10);
        System.out.println("执行删除语句成功，影响的条数为：" + i);
    }
}
```

#### Mybatis中#与$的区别是什么?(面试题)

#是占位符，会替换为?生成预编译SQL(推荐)

$ 是字符串拼接符号,将参数值直接拼接在SQL中

### 新增操作

Mapper

```
@Insert("insert into user(id, username, password, name, age) values(#{id}, #{username}, #{password}, #{name}, #{age})")
public void insert(User user);
```

调用

```
@Test
public void testInsert() {
    User user = new User(null, "admin", "admin", "管理员", 18);
    userMapper.insert(user);
}
```

### 修改操作

Mapper

在MyBatis中，当使用`#{字段名}`占位符时，MyBatis会自动查找传递的对象中的相应属性，并将其值绑定到SQL语句中。

```
@Update("update user set username=#{username}, password=#{password}, name=#{name}, age=#{age} where id=#{id}")
public void update(User user);
```

调用

```
@Test
public void testUpdate() {
    User user = new User(10, "user10", "123456", "牛逼逼", 28);
    userMapper.update(user);
}
```

### 查询操作

Mapper

`@Param`注解用于在接口方法中给传递的形参起名字

说明:基于官方骨架创建的springboot项目中，接口编译时会保留方法形参名，@Param注解可以省略(#{形参名})。

```
@Select("select * from user where username=#{username} and password=#{password}")
public User findUser(@Param("username")String username, @Param("password")String password);
```

调用

```
@Test
public void testFindUser() {
    User user = userMapper.findUser("user10", "123456");
    System.out.println(user);
}
```



## XML映射配置

### 执行复杂SQL语句

​		在Mybatis中，既可以通过注解配置 SQL 语句，也可以通过 XML 配置文件配置 SQL 语句。
默认规则:

1. XML映射文件的名称与Mapper接口名称一致，并且将 XML 映射文件和Mapper接口放置在相同包下(同包同名)

2. XML映射文件的namespace属性为Mapper接口全限定名一致

3. XML映射文件中 sql 语句的 id 与 Mapper 接口中的方法名一致，并保持返回类型一致

   ```
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.lathezero.mapper.UserMapper">
           <!--resultType: 查询返回的单条记录所封装的类型-->
       <select id="findAll" resultType="com.lathezero.pojo.User">
           select id, username, password, name, age from user
       </select>
   </mapper>
   ```

   > 使用注解来映射简单语句会使代码显得更加简洁，但对于稍微复杂一点的语句，Java 注解不仅力不从心，还会让你本就复杂的 SQL 语句更加混乱不堪。 因此，如果你需要做一些很复杂的操作，最好用 XML 来映射语句。

   > 选择何种方式来配置映射，以及认为是否应该要统一映射语句定义的形式，完全取决于你和你的团队。 换句话说，永远不要拘泥于一种方式，你可以很轻松的在基于注解和 XML 的语句映射方式间自由移植和切换。

### 辅助配置

#### 配置XML映射文件的位置：

```
mybatis.mapper-locations=classpath:mapper/*.xml
```

#### mybatis插件:

- MyBatisX
- 提高开发效率