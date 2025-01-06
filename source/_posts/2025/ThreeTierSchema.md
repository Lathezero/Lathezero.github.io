---
title: web开发中的分层解耦
date: 2025-01-02 18:54:04
categories:
  - [后端开发, Java]
  - [开发工具, 框架]
tags: ['SpringBoot', 'Java', '后端开发', '分层解耦', '框架']
description: SpringBoot中对三层架构进行分层与解耦
keywords: SpringBoot, Java, 后端开发, 分层解耦, 框架
---

## 三层架构与解耦

### 三层架构

1. 接收请求、响应数据Controller
2. 逻辑处理 Server
3. 数据访问 Dao 

耦合: 衡量软件中各个层/各个模块的依赖关联程度。

内聚: 软件中各个功能模块内部的功能联系。

软件设计原则: 高内聚低耦合。

|耦合|解耦|
|:---:|:---:|
|![耦合](images/分层解耦/耦合.png)|![解耦](images/分层解耦/解耦.png)|

### 对三层架构实现解耦

1. 控制反转:Inversion 0fControl，简称IOC。对象的创建控制权由程序自身转移到外部(容器)，这种思想称为控制反转。
2. 依赖注入:Dependency Injection，简称DI。容器为应用程序提供运行时，所依赖的资源，称之为依赖注入。
3. Bean对象:IOC容器中创建、管理的对象，称之为Bean。

### 实现

1. 控制反转(DI):

   将Dao 及 Service层的实现类，交给IOC容器管理

   为对象加上 @Component 注解：将这个类产生的 Bean 对象交给 IOC 容器管理

   ```
   @RestController
   public class EmpController {
       @Qualifier("empDaoServiceA") // 默认按类型注入 
   //    @Resource(name = "empDaoServiceB") // 按照名称注入
       @Autowired // 依赖注入
       private EmpService empService;
       @RequestMapping("/listEmp")
       public Result list() {
           List<Emp> empList = empService.listEmp();
           return Result.success(empList);
       }
   }
   ```

   

2. 依赖注入(IOC):

   为Controller 及 Service注入运行时所依赖的对象。

   为对象运行时所依赖的成员变量资源加上 @Autowired 注解：应用程序运行时，会自动的查询该类型的Bean对象，并赋值给该成员变量

   ```
   @Component
   public class EmpDaoServiceA implements EmpService{
       @Autowired
       private EmpDao empDao;
   
       @Override
       public List<Emp> listEmp() {
           List<Emp> empList = empDao.listEmp();
           // 2.对数据进行转化 gender job
           empList.stream().forEach(emp -> {
               emp.setGender(emp.getGender().equals("1") ? "男" : "女");
   
               switch (emp.getJob()) {
                   case "1":
                       emp.setJob("讲师");
                       break;
                   case "2":
                       emp.setJob("班主任");
                       break;
                   case "3":
                       emp.setJob("就业指导");
                       break;
               }
           });
           return empList;
       }
   }
   ```

   ```
   @Repository("DaoA")
   public class EmpDaoA implements EmpDao {
       @Override
       public List<Emp> listEmp() {
           // 1. 加载、解析emp.xml文件
           String filePath = this.getClass().getClassLoader().getResource("emp.xml").getFile();
           List<Emp> empList = XmlParserUtils.parse(filePath, Emp.class);
           return empList;
       }
   }
   ```

总结：

1. Controller 接收请求后向 Service 发起请求，Service 接收请求后向 Dao 请求数据
2. Dao 解析数据后，返回给 Service，Service 接收数据后对数据进行处理并返回给 Controller 
3. 所以要对 Dao 和 Service `控制反转`，要对 Controller 和 Service `依赖注入`



## 规范化

### Bean依赖注入的衍生注解

![Bean注解](images/分层解耦/Bean注解.png)

从上到下分别为：

​		其他 Bean、Controller Bean、 Service Bean、 Dao Bean

开发中除了声明控制器只能用@Cotroller，其他的并不强制要求对应的Bean写对应的注释，不过为了规范，还是有必要的。

@Repository 中实际上已经继承了 @Component 注解

​		![Repository](images/分层解耦/Repository.png)

其他同理

​		![Service](images/分层解耦/Service.png)

​		![RestController](images/分层解耦/RestController.png)

前面声明bean的四大注解，要想生效，还需要被组件扫描注解@ComponentScan扫描。

该注解虽然没有显式配置，但是实际上已经包含在了启动类声明注解 

![SpringBootApplication](images/分层解耦/SpringBootApplication.png)

![SpringBootApplication](images/分层解耦/SpringBootApplication.png)

@SpringBootApplication 中，默认扫描的范围是启动类所在包及其子包

### Autowired依赖注入

基于@Autowired进行依赖注入的常见方式有如下三种：

1. 属性注入

   优点:代码简洁、方便快速开发。

   缺点:隐藏了类之间的依赖关系、可能会破坏类的封装性

![属性注入](images/分层解耦/属性注入.png)

2. 构造器注入

   可以不写无参构造器。如果当前类中只存在一个构造函数，还可以省略@Autowired 注解

   优点:能清晰地看到类的依赖关系、提高了代码的安全性。

   缺点:代码繁琐、如果构造参数过多，可能会导致构造函数臃肿。

   注意:如果只有一个构造函数，@Autowired注解可以省略。

![构造器注入](images/分层解耦/构造器注入.png)

3. setter注入

   优点:保持了类的封装性，依赖关系更清晰。
   缺点:需要额外编写setter方法，增加了代码量。

![setter注入](images/分层解耦/setter注入.png)

### @Autowired 衍生

@Autowired注解，默认是按照类型进行注入的
如果存在多个相同类型的bean，将会报错

解决方法：

1. 使用@Primary
 - 这个注解用于指定当有多个相同类型的bean时，哪一个bean应该被优先考虑。当你通过类型来自动装配（autowire）一个bean时，如果有多个候选者，Spring容器会根据@Primary注解来决定使用哪一个。如果没有指定@Primary，Spring容器可能会抛出异常，因为它不知道应该选择哪一个。
 - 例如，如果你有两个实现了相同接口的bean，你可以在一个bean上使用@Primary，这样当通过接口类型注入时，就会使用这个被标记为@Primary的bean。
2. 搭配@Qualifier使用 @Autowired + @Qualifier
 - 当你想要精确控制Spring容器应该注入哪一个bean时，可以使用@Qualifier注解。它通常与@Autowired注解一起使用，用于消除歧义，当你有多个相同类型的bean时，@Qualifier注解允许你指定注入哪一个具体的bean。
 - 例如，如果你有两个名为"dataSource"的bean，你可以在注入时使用@Qualifier("dataSource1")来指定注入哪一个。
3. 使用@Resource
 - @Resource注解是由Java EE提供的，它也可以用来注入bean，但与@Autowired不同，@Resource注解既可以根据类型注入，也可以根据名称注入。当没有指定name属性时，它会尝试根据字段名或setter方法的参数名来匹配bean。
 - @Resource注解有一个name属性，你可以指定一个bean的名称来注入特定的bean。如果没有指定name属性，它会根据字段名或setter方法的参数名来注入匹配的bean。
 - 例如，如果你有一个名为"dataSource"的bean，你可以在需要注入的地方使用@Resource(name="dataSource")来指定注入这个bean。

@Resource 与 @Autowired 的区别：

1. @Autowired是Spring框架提供的注解，而@Resource是JavaEE规范提供的
2. @Autowired默认是按照类型注入，而Resource默认是按照名称注入