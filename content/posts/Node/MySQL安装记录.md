---
title: "MySQL安装记录"
date: 2020-06-16 14:29:14
draft: false
tags: ['node', '基础', 'mysql']
categories: ['Node']
---

个人下载的是[MySQL Community Server](https://dev.mysql.com/downloads/mysql/)版本，免费不要钱够用。
本次安装的是绿色版，体积小。
### 安装绿色版步骤

1. 下载zip安装包：

![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1617896506272-aaeab5e0-54d5-4a4a-9940-3b302d2b77ee.png#align=left&display=inline&height=151&margin=%5Bobject%20Object%5D&name=image.png&originHeight=302&originWidth=1425&size=53518&status=done&style=none&width=712.5)

2. 解压至非中文路径，管理员模式打开终端，进入mysql的安装路径：

![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1617896668614-8d0d1252-5ee5-49e8-a0b8-c2c8d511f0c7.png#align=left&display=inline&height=78&margin=%5Bobject%20Object%5D&name=image.png&originHeight=155&originWidth=624&size=12403&status=done&style=none&width=312)
![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1617896619206-843f52f0-5b16-40fb-95f4-2b8569cd96b0.png#align=left&display=inline&height=63&margin=%5Bobject%20Object%5D&name=image.png&originHeight=126&originWidth=716&size=9680&status=done&style=none&width=358)

3. 运行安装命令：`mysqld --install`
3. 初始化mysql：`mysqld --initialize --console`，并复制生成的随机登录密码；
3. 启动mysql：`net start mysql`
3. 进入数据库：`mysql -u root -p`，会提示输入密码(输入上面的随机密码)
3. 修改密码：mysql> `alter user 'root'@'localhost' identified by 'root';` 表示将密码修改为`root`
3. 退出数据库：mysql> `exit`
3. 停止mysql：`net stop mysql`

### 配置环境变量
我的电脑-属性-高级系统设置-高级-环境变量-系统变量，点击新建：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1617897116066-69ac9a23-9d4d-442f-b820-d4b81045ffb6.png#align=left&display=inline&height=128&margin=%5Bobject%20Object%5D&name=image.png&originHeight=256&originWidth=1016&size=17098&status=done&style=none&width=508)
变量值为mysql的安装路径。
系统变量-Path，点击编辑，点击新建：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1617897192601-13566c09-09ba-434b-a3ab-5f5a1046866f.png#align=left&display=inline&height=196&margin=%5Bobject%20Object%5D&name=image.png&originHeight=391&originWidth=562&size=23023&status=done&style=none&width=281)
配置完之后，打开终端输入mysql相关命令即可操作mysql。

### 配置文件my.ini
在mysql安装目录下新建文件my.ini，该文件记录了mysql的基础配置：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1617897454209-3c9a011d-8e80-4fec-98bb-bb4ed347dce0.png#align=left&display=inline&height=211&margin=%5Bobject%20Object%5D&name=image.png&originHeight=421&originWidth=459&size=19560&status=done&style=none&width=229.5)
文件内容参考如下：
```css
[mysqld]
# 设置3306端口
port=3306 
# 设置mysql的安装目录（存放地址要改成你的下载路径）
basedir=D:\SoftWare\mysql-8.0.23-winx64
# 设置mysql数据库的数据的存放目录（存放地址要改成你的下载路径）
datadir=D:\SoftWare\mysql-8.0.23-winx64\data
# 允许最大连接数
max_connections=200 
# 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
max_connect_errors=10 
# 服务端使用的字符集默认为UTF8
character-set-server=utf8
# 创建新表时将使用的默认存储引擎 
default-storage-engine=INNODB
# 默认使用“mysql_native_password”插件认证
default_authentication_plugin=mysql_native_password

[mysql]
# 设置mysql客户端默认字符集 
default-character-set=utf8

[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8
```



