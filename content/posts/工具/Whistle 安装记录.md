---
title: "Whistle 安装记录"
date: 2021-01-01T00:00:00+08:00
draft: false
tags: ['基础', '工具']
categories: ['工具']
---

> [whilstle使用文档](http://wproxy.org/whistle/)
> [抓包https请求的解决办法](https://www.cnblogs.com/fafa-coding/p/10832212.html)

1. 安装：npm i whilstle -g
1. 查看是否安装成功：w2 help
1. 运行：w2 start，如图所示：

![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1625131084308-d755079a-c9ee-46d8-8b61-c3686a3172b7.png#height=154&id=ue46db2d3&margin=%5Bobject%20Object%5D&name=image.png&originHeight=154&originWidth=767&originalType=binary&ratio=1&size=15041&status=done&style=none&width=767)

4. 可通过127.0.0.1:8899访问whilstle的配置页面
4. 利用Chrome插件SwitchyOmega，来配置系统代理（不要去控制面板中设置系统代理），安装SwitchyOmega，配置如图所示：

![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1625131241584-46c0736f-182f-42d3-b58a-b9f679044a92.png#height=267&id=uc557f9b4&margin=%5Bobject%20Object%5D&name=image.png&originHeight=267&originWidth=1207&originalType=binary&ratio=1&size=29332&status=done&style=none&width=1207)<br />![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1628073494380-1f0e4735-69c6-4126-92ef-970e84c32332.png#clientId=uc6f70d2a-a27f-4&from=paste&height=212&id=udd1b4a45&margin=%5Bobject%20Object%5D&name=image.png&originHeight=212&originWidth=766&originalType=binary&ratio=1&size=13287&status=done&style=none&taskId=u45271487-7c4e-42a4-850e-4027ae8941b&width=766)

5. Chrome右上角，启用该情景模式，如图所示：

![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1625131278037-ccbd6e4e-4a35-453e-8ae5-bbb2667f5333.png#height=224&id=u607f728c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=224&originWidth=289&originalType=binary&ratio=1&size=11751&status=done&style=none&width=289)

6. 配置完成后，可通过http://local.whistlejs.com/访问whilstle的配置页面
6. whilstle配置信息如图所示

![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1625131450348-0fd883fc-3257-43c7-8da5-2657c7e5bf4a.png#height=139&id=ua046476e&margin=%5Bobject%20Object%5D&name=image.png&originHeight=139&originWidth=909&originalType=binary&ratio=1&size=13836&status=done&style=none&width=909)

8. 点击"HTTPS"，点击下载：

![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1625131502554-b398c8e4-53be-4ef7-b369-cbac24aedd8e.png#height=147&id=u8675c50a&margin=%5Bobject%20Object%5D&name=image.png&originHeight=147&originWidth=343&originalType=binary&ratio=1&size=6142&status=done&style=none&width=343)

9. 安装下载的证书，步骤为：安装证书 - 当前用户 - 将所有证书都放入下列存储 - 受信任的根证书颁发机构 - 确定
9. 证书安装完成后，勾选下列选项，即可对HTTPS请求进行监听：

![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1625131630770-6425a1b0-5e3d-469d-93f9-9b781467e8bb.png#height=532&id=ubcf8f4ff&margin=%5Bobject%20Object%5D&name=image.png&originHeight=532&originWidth=348&originalType=binary&ratio=1&size=20442&status=done&style=none&width=348)

11. 根据上面对switchyOmega、whilstle的配置，即可通过https://wqs.jd.com/访问127.0.0.1:7000

