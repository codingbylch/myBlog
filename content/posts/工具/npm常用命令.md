---
title: "npm常用命令"
date: 2021-01-01T00:00:00+08:00
draft: false
tags: ['基础', 'npm']
categories: ['前端']
---
### 1.查看Npm配置
> npm config list       //查看npm主要配置包含：npm仓库地址，cwd路径，根目录等配置信息


### 2.切换仓库到淘宝镜像
> 一次性：
> npm --registry [https://registry.npm.taobao.org](https://registry.npm.taobao.org) install express    // 修改当前项目的仓库位置
> 永久性：
> npm config set registry [https://registry.npm.taobao.org](https://registry.npm.taobao.org)    //修改全局配置到淘宝镜像
> npm config get registry
> 或  
> npm info express 查看配置是否成功


### 3. 添加cnpm 指令
> npm install -g cnpm --registry=https://registry.npm.taobao.org


### 4.切换回Npm官方
> 发布Npm包时需要先切换回官方地址
> npm config set registry [http://registry.npmjs.org](http://registry.npmjs.org) 


### 5.Npm包到官方仓库
> 先执行login命令登录到官方仓库中。
> npm login
> 切换到生成目录(默认=dist)目录下之后执行，将生成的包推送到官方仓库
> npm publish --access=public //public表示发布的包的访问级别为public。


### 6.安装npm包
> `npm install`
> 缩写为 `npm i`
> 此命令会将包安装到node_modules中，但是不会修改package.json，执行npm install时也不会自动安装。示例：
> npm install @angular/core
> `npm install -g`
> 此命令会将包安装到全局目录中(npm config get prefix所对应的目录)，不修改package.json，执行npm install不会自动安装
> npm install -g @angular/core
> `npm install --save`
> 此命令会将包安装到node_modules中，同时修改package.json文件，添加到dependencies（生产环境）节点。执行npm install 时会自动安装这个包。运行npm install --production或者注明NODE_ENV变量值为production时，会自动下载模块到node_modules目录中。
> npm install --save @angular/core
> `npm install --save-dev`
> 此命令会将包安装到node_modules中，同时修改packgage.json,将包添加到devDependencies（开发环境）节点中，执行npm install 会自动安装这个包到node_modules中，运行npm install --production或者注明NODE_ENV变量值为production时，不会自动下载模块到node_modules目录中。
> npm install --save-dev @angular/core
> `-S 表示 --save`，添加到dependencies节点
> `-D 表示 --save-dev`，添加到devDependencies节点
> `npm i element@2.12.0`，指定插件版本


### 7.查看Npm版本
> npm -v


### 8.查看当前安装的包依赖关系
> npm ls


### 9.卸载包
> npm uninstall

