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

![](https://s2.loli.net/2022/07/11/W58ah16IUuy9wJl.png)

4. 可通过127.0.0.1:8899访问whilstle的配置页面
4. 利用Chrome插件SwitchyOmega，来配置系统代理（不要去控制面板中设置系统代理），安装SwitchyOmega，配置如图所示：

![](https://s2.loli.net/2022/07/11/IDhiWCo149QOJBg.png)<br />
![](https://s2.loli.net/2022/07/11/oVtv689kwWduLG1.png)

5. Chrome右上角，启用该情景模式，如图所示：

![](https://s2.loli.net/2022/07/11/1TJKvhwym3zCbHP.png)

6. 配置完成后，可通过http://local.whistlejs.com/访问whilstle的配置页面
6. whilstle配置信息如图所示

![](https://s2.loli.net/2022/07/11/JueXkhR8PMHzi2p.png)

8. 点击"HTTPS"，点击下载：

![](https://s2.loli.net/2022/07/11/nNB1mtM23djW9pC.png)

9. 安装下载的证书，步骤为：安装证书 - 当前用户 - 将所有证书都放入下列存储 - 受信任的根证书颁发机构 - 确定
9. 证书安装完成后，勾选下列选项，即可对HTTPS请求进行监听：

![](https://s2.loli.net/2022/07/11/cPLr7hx1qfGU4KS.png)

11. 根据上面对switchyOmega、whilstle的配置，即可通过https://wqs.jd.com/访问127.0.0.1:7000

