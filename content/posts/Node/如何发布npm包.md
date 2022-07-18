---
title: "如何发布npm包"
date: 2020-06-16 14:29:14
draft: false
tags: ['node', '基础']
categories: ['Node']
---
> 参考资料：
> 1. [nodejs打包js模块并发布到npm使用或者直接打包到本地使用](https://www.dazhuanlan.com/2020/01/23/5e2924ba1fe90/)
> 1. [整体打包vue组件并发布到npm](https://blog.csdn.net/weixin_43909743/article/details/109267953)
> 1. [使用TypeScript开发项目打包发布到npm](https://segmentfault.com/a/1190000021740976)
> 
源码文件：[TS.zip](https://www.yuque.com/attachments/yuque/0/2021/zip/714353/1620898879428-d888c64e-75ab-4fee-a360-e0527ae5ef2a.zip?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fzip%2F714353%2F1620898879428-d888c64e-75ab-4fee-a360-e0527ae5ef2a.zip%22%2C%22name%22%3A%22TS.zip%22%2C%22size%22%3A49866%2C%22type%22%3A%22application%2Fx-zip-compressed%22%2C%22ext%22%3A%22zip%22%2C%22status%22%3A%22done%22%2C%22taskId%22%3A%22u9b82b55e-7ae6-4878-9539-362ab355f3a%22%2C%22taskType%22%3A%22upload%22%2C%22id%22%3A%22u2c32a23b%22%2C%22card%22%3A%22file%22%7D)

### JS模块发布为npm包
需求：想要在代码中引入自己写的依赖包，并直接使用：
```javascript
import { GuideMiniProgram } from 'codingbylch-demo';
GuideMiniProgram();
```
实现步骤：

1. 下载node和vscode（环境等安装忽略）
1. 初始化package.json，终端输入：`npm init -y`
```json
{
  "name": "node-pub-test", // 修改需要发布到npm上的名称
  "version": "0.1.0", // 版本号，每发布一次都需要更改版本号，从0.1.0开始比较不错
  "description": "", // 对项目的描述
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "", // 作者名
  "license": "ISC"
}
```

3. 若有依赖其它包，npm进行安装并正常在需要使用的地方引入即可
3. 新建一个文件夹demo，创建JS代码文件index.js：
```javascript
import axios from 'axios'
export function sum(a,b){
  axios.get('xxx');
	return a + b;
}
```

5. 添加文件`.npmignore`，类似.gitignore，对不想上传至npm的文件/文件夹进行忽略
5. 打开npm官网，注册npm账号，编写好代码以后，在终端中输入：`npm login`，输入用户名、密码、邮箱。输入npm who am i 若显示用户名，表示已经登录成功
5. 发布到npm：在终端中输入`npm publish`，即可发布到npm
5. 别人如何引入：`npm install xxxxx`，你的npm包就会存在于别人的node_modules中

### 命令扩展

- 撤销已经发布的包

不想在npm包让别人再下载你开发的依赖包时，但尽量避免这种情况，在终端输入此命令：
`npm unpublish XXXXXX`

### 打包后的文件如何配置
源代码可能包含ES6/ES7等语法，考虑兼容性，会利用webpack/babel进行打包编译，生成打包后的文件（如bundle.js）。打包后的文件被引入时可能提示导出的是空对象，针对此情况webpack需要做如下配置：
```javascript
// webpack.config.js
const path = require("path");
module.exports = {
  entry: "./src/index.ts",
  mode: "production",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "gotoWxMini.js", // 打包后生成的文件名称
    libraryTarget: "umd", // 打包后文件的导出形式
    libraryExport: "default", // 对外暴露default属性，就可以直接调用default里的属性
    library: "GOTOWX", // 类库名，主要用于直接引用的方式(比如使用script 标签)
    globalObject: "this", // 定义全局变量, 兼容node和浏览器运行, 避免出现"window is not defined"的情况
    environment: {
      arrowFunction: false,
      const: false,
    },
  }
  //......
}
```
根据以上配置，然后运行npm run build，打包后的文件可被如此使用：
```javascript
// 法1
import gotoWx from 'gotoWxMini.js'
gotoWx()
// 法2
<script src="./gotoWxMini.js" />
GOTOWX()
```
