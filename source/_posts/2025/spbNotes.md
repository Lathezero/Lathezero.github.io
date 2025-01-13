---
title: sprintboot notes
date: 2025-01-12 22:07:27
categories: 
   - [后端开发, Java]
   - [开发工具, 框架]
tags: [Java, SpringBoot]
description: SpringBoot框架
keywords: Java, SpringBoot
---

### 框架

1. 创建空项目，sprintboot模块
2. 使用三层架构设计
3. 创建控制层、逻辑层接口、逻辑实现类、持久层
4. 准备封装的对象和返回对象

### 手动封装

1. 使用Results和Result

   ```
   @Results({
   	@Result(column = "create_time", property = "createTime"),
   	@Result(column = "update_time", property = "updataTime"),
   })
   @select("select id, name, create_time, update_time, from tlias order by update_time desc")
   public List<Dept> findAll();
   ```

2. 起别名

   ```
   @select("select id, name, create_time createTime, update_time updateTime, from tlias order by update_time desc")
   public List<Dept> findAll();
   ```

3. 开启驼峰命名

   ```
   mybatis:
     configuration:
       log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
       map-underscore-to-camel-case: true
   ```

### 使用nginx部署前端

1. 通过HttpServletRequest对象获取

   ```
   @DeleteMapping("/depts")
   public Result delete(HttpServletRequest request) {
   	String idstr = request.getParameter("id");
   	int id = Integer.parseInt(idstr);
   	deptService.deleteByid(id);
   	System.out.println("根据ID删除部门:"+id);
   ```

   

2. 后端使用@RequestParam接收参数

   ```
   @DeleteMapping("depts")
   public Result deleteByid(@RequestParam("id") Integer deptId) {
       deptService.deleteByid(deptId);
       System.out.println("删除部门数据：" + deptId);
       return Result.success();
   }
   ```

   > 使用@RequestParam默认会绑定参数，且不能为空，可以设置`required`属性为`false`代表参数可选

3. 当前端请求的参数名与服务端方法形参一样时，可省略@RequestParam注解 *

   ```
   @DeleteMapping("depts")
   public Result deleteByid(Integer id) {
       deptService.deleteByid(id);
       System.out.println("删除部门数据：" + id);
       return Result.success();
   }
   ```


### 接收前端的 JSON 数据

```
@PostMapping("depts")
public Result insert(@RequestBody Dept dept) {
    deptService.insert(dept);
    System.out.println("新增部门数据：" + dept.getName());
    return Result.success();
}
```

>  @RequestBody注解 接收JSON格式数据并封装到一个对象当中（注：该对象中必须有一个与形参相同的属性）

```
public class Dept {
    private Integer id;
    private String name;
    private LocalDateTime createTime;
    private LocalDateTime updateTime;
```

### 接收路径参数

```
@GetMapping("depts/{id}")
public Result getById(@PathVariable("id") Integer id) {
    Dept dept = deptService.getById(id);
    System.out.println("查询部门数据：" + dept.getName());
    return Result.success(dept);
}
```

> @PathVariable注解 将路径参数赋值给形参

> 当路径名与形参一致时可以省略路径名
>
> ```
> @GetMapping("depts/{id}")
> public Result getById(@PathVariable Integer id) {
>     Dept dept = deptService.getById(id);
>     System.out.println("查询部门数据：" + dept.getName());
>     return Result.success(dept);
> }
> ```
