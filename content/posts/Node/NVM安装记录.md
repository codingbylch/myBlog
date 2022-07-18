---
title: "NVM安装记录"
date: 2020-06-16 14:29:14
draft: false
tags: ['node', '基础', '软件安装']
categories: ['Node']
---

nvm作用：用于管理node版本，实现node版本的快速切换。
安装步骤：

1. 去github下载nvm-windows，nvm本身是不支持windows，不过github上有[nvm-windows](https://github.com/coreybutler/nvm-windows/releases)，下载后双击安装，安装的目录不能出现中文、空格，否则后续安装node会报错。
1. 安装完成以后，打开终端，输入nvm，若有指令显示则表示安装成功
1. 对应指令如下：

![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1616916719380-d413de9d-8364-4211-9d49-14abcd8e8335.png#align=left&display=inline&height=309&margin=%5Bobject%20Object%5D&name=image.png&originHeight=617&originWidth=367&size=27128&status=done&style=none&width=183.5)

4. 先安装淘宝镜像，否则下载node和npm会很慢。

`nvm node_mirror https://npm.taobao.org/mirrors/node/`
`nvm npm_mirror https://npm.taobao.org/mirrors/npm/`

5. 输入`nvm list available`查看可用的node版本。
5. 输入`nvm install latest`下载最新版，`nvm install lts`下载稳定版。
5. `nvm use 版本号`使用对应版本。
