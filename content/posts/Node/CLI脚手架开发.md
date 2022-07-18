---
title: "CLI脚手架开发"
date: 2020-06-16 14:29:14
draft: false
tags: ['node', '基础']
categories: ['Node']
---

> 参考代码：[02_learn_cli.7z](https://www.yuque.com/attachments/yuque/0/2021/7z/714353/1623825864550-97822779-432e-4414-924a-ee0e6ded6d71.7z?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2F7z%2F714353%2F1623825864550-97822779-432e-4414-924a-ee0e6ded6d71.7z%22%2C%22name%22%3A%2202_learn_cli.7z%22%2C%22size%22%3A14719%2C%22type%22%3A%22%22%2C%22ext%22%3A%227z%22%2C%22status%22%3A%22done%22%2C%22taskId%22%3A%22ue911b061-54be-4318-872c-e547c13cbfd%22%2C%22taskType%22%3A%22upload%22%2C%22id%22%3A%22u5f59ee41%22%2C%22card%22%3A%22file%22%7D)

## 开发脚手架工具
最终要实现的效果：
cli_name为脚手架名称。
安装：npm install cli_name -g
该脚手架支持Vue，具有以下结构：

- 常用目录结构
- vue.config.js
- axios
- vue-router
- vuex
- 其它功能：
   - 添加组件
   - 添加路由、页面
   - 添加vuex子模块

可使用的相关命令：


整体结构：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1623233352351-75ce2984-7040-47cb-ba61-def1b37c9b95.png)
## 配置终端相关命令
**首先，如何让自己的终端命令可生效，如在终端输入**`**why cli**`

1. 新建文件夹，并在终端输入：`npm init -y`，生成package.json文件
1. package.json文件里可知入口文件为index.js，创建index.js
```javascript
// index.js
#!/usr/bin/env node // 找到当前环境下的node, 交给node来执行, 一般是固定的
console.log("hello world");
```

3. 在package.json中添加bin属性：
```javascript
// ......
"bin": {
  "why": "index.js"
}
```

4. 在终端输入：`npm link`，表示将bin与环境中的bin进行合并链接
4. 终端：`why`

开发脚手架通常会用到第三方库[commander](https://github.com/tj/commander.js/blob/HEAD/Readme_zh-CN.md)，利用该库可以添加option、command等，安装步骤如下：

1. 安装：`npm install commander`
1. 引用：`const program = require("commander"); program.parse(process.argv);`
1. 使用：`why --help`，commander自带的命令

### 配置Options
options即可选参数，例如-d，-g等等，示意图如下：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1623161673654-f8e6e46b-b79a-4d13-bdd2-dd3ce6284090.png)
配置代码为：
```javascript
// index.js
#!/usr/bin/env node
const program = require("commander"); // commander是一个第三方库，很多脚手架用到这个库
// 利用commander提供的api定义查看版本号的命令
program.version(require("./package.json").version, "-v, --version");

// 增加自己的options
program.option("-c --command", "description");
program.option("-d --dest <dest>", "a destination folder，如：-d /src/components");
program.option("-f --framework <framework>", "select a framework type");

program.on("--help", function () { // 试试就知道了
  console.log(" ");
  console.log("Other:");
  console.log("other options~");
});

program.parse(process.argv); // process.argv即why xxx的xxx内容，然后交由program解析，一般写在最后
```
按以上配置以后，就可以使用"why -c"命令了，不过此时终端啥也没有，还需要进一步配置。

---

另外，可以将以上部分代码进行封装，抽离出来使得逻辑更加清晰，再导入[help.js](https://www.yuque.com/attachments/yuque/0/2021/js/714353/1623164007183-939acf06-611d-43d0-b031-c3f1eef6de18.js?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fjs%2F714353%2F1623164007183-939acf06-611d-43d0-b031-c3f1eef6de18.js%22%2C%22name%22%3A%22help.js%22%2C%22size%22%3A279%2C%22type%22%3A%22text%2Fjavascript%22%2C%22ext%22%3A%22js%22%2C%22status%22%3A%22done%22%2C%22taskId%22%3A%22u47a0e4a6-8fca-4329-85fe-308bf6a49b9%22%2C%22taskType%22%3A%22upload%22%2C%22id%22%3A%22uf7b794ed%22%2C%22card%22%3A%22file%22%7D)：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1617620807301-3bb057c8-2018-4819-ac08-b20c1b140b7b.png#height=157&id=qgSnh&margin=%5Bobject%20Object%5D&name=image.png&originHeight=313&originWidth=339&originalType=binary&ratio=1&size=17367&status=done&style=none&width=169.5)
```javascript
// index.js
#!/usr/bin/env node
const helpOptions = require('./lib/core/help')
const program = require("commander");
// 查看版本号
program.version(require("./package.json").version, "-v, --version");
helpOptions() // 封装导入，-- 帮助和可选信息
program.parse(process.argv);
```
这样做以后，在终端中输入`why --help`，可得到：
![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1617632785651-6b9f165b-b3c3-4859-92e1-063455efc2e6.png#height=184&id=w8Ti1&margin=%5Bobject%20Object%5D&name=image.png&originHeight=367&originWidth=726&originalType=binary&ratio=1&size=32279&status=done&style=none&width=363)

### 配置Commands
#### 配置命令：why create demo1，即创建某个项目的指令
（指令的功能：从仓库下载，执行npm install）
创建步骤：
创建lib/core/create.js，用于存放创建的指令，代码如下：
```javascript
// create.js
const program = require("commander");
const { createProjectAction } = require("./actions.js");
const createCommands = () => {
  program
    .command("create <project> [others...]") // 指令的名称，<> [] 这些program会自动识别的
    .description("clone repository into a folder") // 对指令功能的描述
    .action(createProjectAction); // 传入一个回调函数，在输入命令后会执行该函数
};

module.exports = createCommands;
```
该命令所对应的action里的回调函数被重新封装了一下，存放在core/actions.js中：
```javascript
// actions.js 保存action函数的回调 逻辑方法
const { promisify } = require("util"); // 引入promisify, 让download的回调能使用Promise形式
const download = promisify(require("download-git-repo")); // 从节点下载并提取一个git存储库
const { vueRepo } = require("../config/repo-config"); // 引入仓库地址
const { commandSpawn } = require("../utils/terminal"); // 封装第二步，引入

// callback => promisify(函数) => Promise => async await
const createProjectAction = async (project) => {
  // 1.clone项目
  await download(vueRepo, project, { clone: true });
  // 2.执行npm install
  const command = process.platform === 'win32' ? 'npm.cmd' : 'npm'; // 在windows下npm的执行名不同
  await commandSpawn(command, ["install"], { cwd: `./${project}` });
  // 3.自动执行npm run serve
  await commandSpawn(command, ["run", "serve"], { cwd: `./${project}` });
  // 4.自动打开浏览器
  open("http://localhost:8080/"); // 需在终端执行: npm install open，然后引入。去掉上面的await，这一步才不会被阻塞
};

module.exports = {
  createProjectAction,
};
```

1. clone项目：利用到`[download-git-repo](https://www.npmjs.com/package/download-git-repo/v/3.0.2)`，是用于下载一个git仓库。若出现128等错误，可能是地址配置有误，可参考[这篇文章](https://segmentfault.com/q/1010000012493731)。第一个参数就是类型[github/gitlab/Bitbucket]:[账户名]/[仓库名]。这个依赖包不支持Promise，所以引入promisify的依赖包进行包装让其支持Promise；

![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1617634513842-58d6b1aa-d436-48be-baad-50f45ccea5c9.png#height=115&id=O3ZTd&margin=%5Bobject%20Object%5D&name=image.png&originHeight=230&originWidth=732&originalType=binary&ratio=1&size=73000&status=done&style=none&width=366)
其它文件见下面的代码文件。
完成上述代码，得到的效果即：在终端输入why create demo01，即可自动去对应仓库（coderwhy老师的模板仓库）拉取并创建demo01文件夹，并自动运行npm install命令。（不过这里我项目报错找不到淘宝镜像的某个包）

2. 

