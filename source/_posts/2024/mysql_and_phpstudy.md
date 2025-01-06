---
title: 解决 phpstudy 和本地 mysql 不兼容问题
date: '2024-12-23'
categories: 
  - [后端开发, 数据库, MySQL]
  - [问题解决, 环境配置]
tags: ['MySQL', 'phpstudy', '后端开发']
description: 解决 phpstudy 和本地 mysql 不兼容问题
keywords: MySQL, phpstudy, 后端开发
---

## 首先正常安装并且初始化 mysql

1. ### 先删除 mysql 的服务，使用 cmd 管理员模式输入以下指令

   ```bash
   sc delete mysql
   ```

   成功后弹出 "[SC] DeleteService SUCCESS"

   注：若安装 mysql 后没有运行 ```bash mysqld -install```命令，则跳过第一步

2. ### 注册新的 mysql 服务

   1. 打开 cmd 管理员模式，导航到 mysql 的 bin 文件目录，例：

      ```bash
      cd D:\mysql-9.1.0-winx64\bin
      ```

      或者直接打开 bin 文件，然后在上面文件路径中输入 cmd 然后进入到 cmd 界面

   2. 注册 mysql 服务，并且为服务起一个新名字，例：

      ```bash
      mysqld -install mysql9
      ```

      成功后弹出 "Service successfully installed"

3. ### 试运行 mysql

   输入命令：

   ```bash
   net start mysql9
   ```