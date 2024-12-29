---
title: Maven 配置与创建
date: '2024-12-16'
categories: 
  - [后端开发, Java]
  - [开发工具, 构建工具]
tags: ['Maven', 'Java', '后端开发', '项目管理']
description: Maven 基础配置与创建
keywords: Maven, Java, 后端开发, 项目管理
---

# Maven 配置与创建

## 配置Maven文件：

下载maven bin-zip文件，解压。

### 官网：

```plaintext
http://maven.apache.org/
```



1. 修改conf/settings.xml 中的 <localRepository>(大概第53行) 标签内容为maven包路径位置，例：

```xml
<localRepository>D:\apache-maven-3.9.9\</localRepository>
```

2. 配置阿里云私服：修改conf/settings.xml 中的 <mirrors> 标签内容，添加如下内容：

```xml
<mirror>
    <id>alimaven</id>
    <name>aliyun maven</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    <mirrorOf>central</mirrorOf>
</mirror>
```

3. 配置环境变量：复制maven包下的bin地址，添加到path环境变量

## 在idea中配置maven：

1. 打开 idea => File | Settings | Build, Execution, Deployment | Build Tools | Maven

2. Maven home path: 选择 maven 包路径，例：

```plaintext
D:\apache-maven-3.9.9\
```

3. User settings file: 选择 maven 包中的conf/settings.xml,例：

```plaintext
D:\apache-maven-3.9.9\conf\settings.xml
```

4. 把User settings file 和 Local repository 勾选上，点击 Apply 和 OK

## 创建maven项目：

1. 创建一个空项目
1. 点击右上角齿轮 => Projext Structure
1. SDK：选择你的 jdk

## 创建maven模块：

同上

1. 新建项目，选择Maven Archetype，Name、Location随意
2. Archetype 选择 quickstart
3. 下面Advanced Settings：
   1. Groupld：组织名称
   2. Artifactld：模块名称

## 使用

![mavenuse](images/maven/mavenuse.png)