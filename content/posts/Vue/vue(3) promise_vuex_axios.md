---
title: "vue(3) promise_vuex_axios"
date: 2021-03-25T00:00:00+08:00
draft: false
tags: ['Vue', 'coderwhy', '基础']
categories: ['前端']
---

### 一. Promise
[MDN文档Promise](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)
简单的实现：[mypromise.js](https://www.yuque.com/attachments/yuque/0/2021/js/714353/1614836521074-54635c7b-ff68-459f-b99f-fe4c47e4f250.js?_lake_card=%7B%22uid%22%3A%221614836520986-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fjs%2F714353%2F1614836521074-54635c7b-ff68-459f-b99f-fe4c47e4f250.js%22%2C%22name%22%3A%22mypromise.js%22%2C%22size%22%3A2383%2C%22type%22%3A%22text%2Fjavascript%22%2C%22ext%22%3A%22js%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%2268fWw%22%2C%22card%22%3A%22file%22%7D)
具体实现参考：[从0到1实现Promise](https://segmentfault.com/a/1190000016550260)
Promise是异步编程的一种解决方案。我们封装一个网络请求的函数，因为不能立即拿到结果，往往我们会传入另外一个函数，在数据请求成功时，将数据通过传入的函数回调出去。若请求十分复杂，会出现回调地狱。
![](https://s2.loli.net/2022/07/12/Atcmpz9oU1Xi6Ya.png)
代码难看而且不容易维护。Promise以一种更加优雅的方式来进行这种异步操作。

#### 1.1. Promise的基本使用
![](https://s2.loli.net/2022/07/12/Pzy2AEsF6Td5UQw.png)
Promise有3种状态：

- pending：等待状态，比如正在进行网络请求，或者定时器没有到时间。
- fulfill：满足状态，当我们主动回调了resolve时，就处于该状态，并且会回调.then()
- reject：拒绝状态，当我们主动回调了reject时，就处于该状态，并且会回调.catch()

语法：`new Promise(executor)`，其中`executor`是一个**双参函数**，参数为`resolve和reject`，这2个参数也是函数，作为参数传递进来，所以这2个函数被称为回调函数。
```javascript
const myFirstPromise = new Promise((resolve, reject) => {
  // 执行异步操作，最终调用以下任一操作:
  //
  //   resolve(someValue)        // fulfilled
  // or
  //   reject("failure reason")  // rejected
});
```
一般使用如下：
![](https://s2.loli.net/2022/07/12/675Q3vkTDA12KeV.png)
```javascript
const p = new Promise((resolve) => {
  setTimeout(() => {
    console.log("模拟获取数据");
    let data = { name: "oppo", age: 18 };
    resolve(data);
  }, 2000);
});

let myobj = {};
p.then((data) => {
  myobj = data;
}).catch((err) => console.log(err));

console.log(myobj); 
```

- 如何将异步操作放入到promise中
- (resolve, reject) => then/catch

#### 1.2. Promise的链式调用
Promise.resovle()：将数据包装成Promise对象，并且在内部回调resolve()函数
Promise.reject()：将数据包装成Promise对象，并且在内部回调reject()函数

链式调用 | 简写：
![](https://s2.loli.net/2022/07/12/CXSKrihkEpRcAmJ.png)
![](https://s2.loli.net/2022/07/12/XDdTrxEsGwqAuc5.png)
#### 1.3. Promise的all方法
详见MDN文档。
### 二. Vuex
Vuex的API：[https://vuex.vuejs.org/zh/api/](https://vuex.vuejs.org/zh/api/)
别人的子模块的组织方式参考：[redux-module-menu.js](https://www.yuque.com/attachments/yuque/0/2021/js/714353/1614745087810-4b96f68d-3503-455d-ad53-10deade6ed0e.js?_lake_card=%7B%22uid%22%3A%221614745087727-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fjs%2F714353%2F1614745087810-4b96f68d-3503-455d-ad53-10deade6ed0e.js%22%2C%22name%22%3A%22redux-module-menu.js%22%2C%22size%22%3A1491%2C%22type%22%3A%22text%2Fjavascript%22%2C%22ext%22%3A%22js%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22ab78o%22%2C%22card%22%3A%22file%22%7D)
#### 2.1. 什么是状态管理
简单说，就是管理多个组件共享同一个变量会用到。
![](https://s2.loli.net/2022/07/12/b5Xt12pnrVKBUMl.png)
#### 2.2. Vuex的基本使用

1. 安装：`npm i vuex -S`
1. 创建src/store/index.js，并写代码：
```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
const store = new Vuex.Store({
  state:{ // 定义需要管理的状态
  	count:66 
  },
  mutations:{ // 相当于methods，对state进行操作，执行同步操作，可被devtools捕捉
  	increment(state){
      state.count += 1; 
    },
    decrement(state){
    	state.count -= 1;
    }
  },
  getters:{ // 获取一些state变异后的状态，相当于computed
  	// 示例在下面
  },
  actions:{}, // 执行一些异步操作
  modules:{},
})
```

3. 在组件中使用：
```javascript
  computed: {
    count() {
      return this.$store.state.count;
    }
  },
  methods: {
    increment() {
      this.$store.commit('increment');
    },
      decrement() {
        this.$store.commit('decrement');
      }
  }
```

- state -> 直接修改state(错误)
- mutations -> devtools

#### 2.3. 核心概念

- state -> 单一状态树

状态保存在一个Store对象中，这样管理、维护方便。

- getters ->
```javascript
getters:{
	greaterAgesStus: state => { // 获取年龄不小于20的学生列表
    return state.studuents.filter(s => s.age>=20)
  },
  greaterAgesCount: (state, getters) => { // 可传入第2个默认参数，即getters对象
    return getters.greaterAgesStus.length;
  },
  stuByID: state => { // 若要传入参数，则需在返回的函数中传入参数
    return id => {
      return state.students.find(s => s.id === id);
    }
  }
}

// 通过属性访问，会暴露为 store.getters 对象
store.getters.doneTodos // -> [{ id: 1, text: '...', done: true }]
// 在其他组件中使用：
computed: {
  doneTodosCount () {
    return this.$store.getters.doneTodosCount
  }
}
// 通过方法访问，传参数时：
store.getters.getTodoById(2) // -> { id: 2, text: '...', done: false }
```

- mutations ->

如上，第一个参数：state
若想携带额外参数，被称为是mutation的载荷(Payload)
![](https://s2.loli.net/2022/07/12/8HGBAtxgbsZiIlF.png)
若参数不止一个，则payload可以是一个对象：
![](https://s2.loli.net/2022/07/12/oi7ecwxpBSUIzvC.png)
另一种commite提交的风格：
![](https://s2.loli.net/2022/07/12/ZJlpP67usIcFekw.png)
**响应规则：**
若想给state中的对象添加新属性，需要如下操作：

- 方法一：使用Vue.set(obj, 'newProp', 123)
- 方法二：用新对象给旧对象赋值

![](https://s2.loli.net/2022/07/12/n7GIro5XORC8Ete.png)
**Mutation常量类型 **
当需要更新的状态变多，mutations中的方法也随之变多，变得不好管理和使用。
可以使用一些常量来替代mutation事件的类型，并将其放入到单独的文件中，便于管理使用。就是抽离方法名到一个单独文件中，然后都引入这个文件。
![](https://s2.loli.net/2022/07/12/Yn4tQ6pwoeI1ZGS.png)
![](https://s2.loli.net/2022/07/12/3smQrlDyzWu45XF.png)

- actions -> 异步操作(Promise)

mutaions中存放的是同步的方法，因为devtools工具可以捕捉mutations的快照，只能捕捉同步而无法捕捉异步方法，无法追踪异步操作何时完成。因此，在action中进行异步操作，如网络请求等。
![](https://s2.loli.net/2022/07/12/MTmjLHYGWpsRuCq.png)
其中的context，指的是与store对象具有相同方法和属性的对象，因此可以context.commit('increment')、context.state
定义了actions中的方法，在组件中如何调用呢？使用dispatch，同样也支持传递参数payload
```javascript
// 子组件
methods:{
  increment(){
    this.$store.dispatch('increment', {count:5});
  }
}
// store/index.js
mutations:{
	increment(state,payload){
    state.count += payload.count;
  }
},
actions:{
  increment(context,payload){ // 传参
    setTimeout(()=>{ // 模拟异步而已
    	context.commit('increment',payload);
    },2000)
  }
}
```
可以将异步操作放入actions中：
```javascript
actions: {
  increment(context) {
    return new Promise(resolve => {
      setTimeout(() => {
        const data = { school: 'oppo' };
        context.commit('increment', data);
        resolve();
      }, 2000);
    });
  }
},
// 子组件调用
methods:{
  increment(){
    this.$store.dispatch('increment').then(()=>console.log('ok'););
  }
}
```

- modules

当管理的状态越来越多时，store对象就会变得臃肿，如何解决这个问题？
Vuex允许我们将store分割成模块（Module），每个模块拥有自己的state、mutations、actions、getters等
那如何组织这样的结构？如图所示：
![](https://s2.loli.net/2022/07/12/n2aOT3X5v6yVMHK.png)
在组件中如何调用呢？很简单，和之前一样利用this.$store直接调用对应方法。
**子模块中actions的写法**：（state和mutations写法与上文一致）
```javascript
// 接收一个context参数对象
const moduleA = {
    // ......
    actions: {
        incrementRootSum({ state, commit, rootState }) { // 第三个参数是根模块节点的状态
            if ((state.count + rootState.count) % 2 === 1) {
                commit('increment');
            }
        }
    },
    // ......
};
```
**子模块中getters的写法：**
```javascript
const muduleA = {
  // ...
  getters: {
    sumWithRootCount(state, getters, rootState) { // 第二个参数是getters对象
      return state.count + rootState.count;
    }
  }
}
```
#### 2.4. 目录组织方式
![](https://s2.loli.net/2022/07/12/ynlfo1Wup4Rdi25.png)
### 三. 网络请求封装
#### 3.1. 网络请求方式的选择
传统的ajax是基于XHR的，如果直接使用XHR，配置、调用十分麻烦，有人封装了jQuery-ajax。
vue开发中不用jQuery-ajax，因为引入重量级jQuery，代码更多。
vue官方推出了Vue-resource，但在vue2.0之后就去掉了，不再更新。
目前：axios（基于XHR），官网API：[http://www.axios-js.com/zh-cn/docs/](http://www.axios-js.com/zh-cn/docs/)
别人封装的小参考：[关同学的axios封装.zip](https://www.yuque.com/attachments/yuque/0/2021/zip/714353/1614848816431-25ddbdc0-4e46-42a4-9c0c-7313fd4c5896.zip?_lake_card=%7B%22uid%22%3A%221614848816327-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fzip%2F714353%2F1614848816431-25ddbdc0-4e46-42a4-9c0c-7313fd4c5896.zip%22%2C%22name%22%3A%22%E5%85%B3%E5%90%8C%E5%AD%A6%E7%9A%84axios%E5%B0%81%E8%A3%85.zip%22%2C%22size%22%3A1083%2C%22type%22%3A%22application%2Fx-zip-compressed%22%2C%22ext%22%3A%22zip%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22bXUdo%22%2C%22card%22%3A%22file%22%7D)
#### 3.2. axios的基本使用
在项目开发中：

1. 安装：`npm i axios`
1. 引入：`import axios from 'axios'`
1. 使用：[axios基本使用.js](https://www.yuque.com/attachments/yuque/0/2021/js/714353/1614755346560-8f7006c9-850a-4042-a57e-834d7408a253.js?_lake_card=%7B%22uid%22%3A%221614755346469-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fjs%2F714353%2F1614755346560-8f7006c9-850a-4042-a57e-834d7408a253.js%22%2C%22name%22%3A%22axios%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.js%22%2C%22size%22%3A1392%2C%22type%22%3A%22text%2Fjavascript%22%2C%22ext%22%3A%22js%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22hSECW%22%2C%22card%22%3A%22file%22%7D)

#### 3.3. axios的相关配置
在开发中，很多参数都是固定的，这时候可以做一些参数抽取，避免重复。
默认axios实例的配置：
```javascript
axios.defaults.baseURL = 'https://api.example.com';
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
```
自定义实例默认值：
```javascript
// Set config defaults when creating the instance
const instance = axios.create({ // axios的创建实例
  baseURL: 'https://api.example.com'
});

// Alter defaults after instance has been created
instance.defaults.baseURL = 'http://www.jd.com';
instance.defaults.headers.common['Authorization'] = AUTH_TOKEN;
```
#### 3.4. axios的创建实例
如上。
为什么要创建axios实例？从axios模块导入axios，使用的实例是默认的实例，给该实例进行一些默认配置，该配置就被固定下来。后续开发若想有不一样的配置，就可以创建新的axios实例，并进行相应配置。
举例：
```javascript
axios.defaults.baseURL = 'http://www.baidu.com';
const Instance = axios.create({
	baseURL:'http://www.jd.com'
})
```
#### 3.5. axios的封装
axios的基本封装。在项目中可灵活再封装axios。
```javascript
import originalAxios from 'axios';

export default function axios(options) {
    return new Promise((resolve, reject) => {
        // 1. 创建axios实例
        const instance = originalAxios.create({ // 这样写个人感觉不太好，每次调用都要创建新实例
            baseURL: '',
            timeout: 5000,
            headers: ''
        });

        // 2.传入对象进行网络请求
        instance(options)
            .then(res => resolve(res))
            .catch(err => reject(err));
    });
}
// 可再次简化：
export default function axios(options) {
  // 1. 创建axios实例
  const instance = originalAxios.create({ // 这样写个人感觉不太好，每次调用都要创建新实例
    baseURL: '',
    timeout: 5000,
    headers: ''
  });
  return instance(options); // 因为axios实例返回的就是Promise对象嘛
}
// ps:上面包装了一个new Promise, 若以后axios不在维护并出现了新的包代替，就可以在这里座一层封，
// 而组件中的代码则无需更换
```
#### 3.6. 请求里的配置选项
具体解释：[http://www.axios-js.com/zh-cn/docs/#请求配置](http://www.axios-js.com/zh-cn/docs/#请求配置)
```javascript
const configOption = {
  url: "/user", // 请求服务器的url
  method: "get", // 创建请求的方法, 默认get
  baseURL: "http://www.jd.com/api/", // 自动加在url前面
  transformRequest: [(data, headers) => data], // 发送请求前对数据进行处理, 只适用于PUT/POST/PATCH
  transformResponse: [(data) => data], // 传递给then/catch之前修改响应数据
  headers: { "X-Requested-With": "XMLHttpRequest" },  //自定义请求头
  params: { ID: 12345 }, // 指定url参数
  paramsSerializer: () => {}, // 负责 `params` 序列化的函数
  data: {}, // 作为请求主体被发送的数据, 只适用于PUT/POST/PATCH方法
  timeout: 5000, // 指定请求超时时间，请求超时则中断请求
  withCredentials: false, // 跨域请求时是否需要使用凭证
  adapter: () => {}, // 允许自定义处理请求
  auth: {}, // 表示应该使用 HTTP 基础验证，并提供凭据
  responseType: "json", // 服务器响应的数据类型
  responseEncoding: "utf8", // 表示用于解码响应的编码
  xsrfCookieName: "XSRF-TOKEN", // 用作 xsrf token 的值的cookie的名称
  xsrfHeaderName: "X-XSRF-TOKEN",// XSRF令牌值的http标头的名称
  onUploadProgress: () => {}, // 允许为上传处理进度事件
  onDownloadProgress: () => {}, // 允许为下载处理进度事件
  maxContentLength: 2000, // 定义允许的响应内容的最大尺寸
  validateStatus: () => {}, // 定义对于给定的HTTP响应状态码的promise状态是resolve或reject  
  maxRedirects: 5, // 定义在 node.js 中 follow 的最大重定向数目
  socketPath: null, // 定义了在node.js中使用UNIX套接字
  httpAgent: new http.Agent({ keepAlive: true }), // 定义在执行http时使用的自定义代理
  httpsAgent: new https.Agent({ keepAlive: true }), // 定义在执行https时使用的自定义代理
  proxy: { // 定义代理服务器的主机名称和端口
    host: "127.0.0.1",
    port: 9000,
    auth: {
      username: "mikeymike",
      password: "rapunz3l",
    },
  },
  cancelToken: new CancelToken(function (cancel) {}), // 指定用于取消请求的 cancel token
};

```
#### 3.7. axios的拦截器
拦截器有什么作用？在请求发送之前、得到响应之后，可以做一些处理。
有请求拦截器、响应拦截器。
```javascript
// 发送请求拦截器
instance.interceptors.request.use(
    config => {
        console.log('');
      // 1.当发送请求时，在页面中添加一个loading组件，作为动画；
      // 2.某些请求要求用户登录，判断用户是否有token，没有则跳转到loading页面
      // 3.对请求参数进行序列化
        return config;
    },
    err => {
        console.log('');
      // 1.比如请求超时，可以将页面跳转到一个错误页面中
        return err;
    }
);
// 得到响应拦截器
instance.interceptors.response.use(
    response => {
        console.log('');
      // 1.主要是对数据进行过滤
        return response;
    },
    err => {
        console.log('');
      // 1.根据status判断报错的错误码，跳转到不同的错误提示页面
        return err;
    }
);
```
PS：[axios, ajax和fetch的比较](http://www.axios-js.com/zh-cn/blogs/)
### 四. 项目开发
首先是用脚手架生成初始项目，前提是已经安装了node/npm/vue脚手架：
`vue create 项目名称`
然后划分目录结构。
#### 4.1. 划分目录结构
![](https://s2.loli.net/2022/07/12/QcSZEOymoRx6pku.png)
#### 4.2. 引用了两个css文件
在src/asserts/css中引入2个css文件：

- normalize.css：github上下载[normalize.css](https://www.yuque.com/attachments/yuque/0/2021/css/714353/1615126062364-72854a43-461d-40a8-849f-7c7ea53b3de1.css?_lake_card=%7B%22uid%22%3A%221615126061317-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fcss%2F714353%2F1615126062364-72854a43-461d-40a8-849f-7c7ea53b3de1.css%22%2C%22name%22%3A%22normalize.css%22%2C%22size%22%3A6138%2C%22type%22%3A%22text%2Fcss%22%2C%22ext%22%3A%22css%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22nXdgP%22%2C%22card%22%3A%22file%22%7D)
- base.css：自己写的，定义项目基本的css[base.css](https://www.yuque.com/attachments/yuque/0/2021/css/714353/1615126065175-7ecdd26f-3314-41fe-af97-2d9cfe0c8cb3.css?_lake_card=%7B%22uid%22%3A%221615126064188-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fcss%2F714353%2F1615126065175-7ecdd26f-3314-41fe-af97-2d9cfe0c8cb3.css%22%2C%22name%22%3A%22base.css%22%2C%22size%22%3A1120%2C%22type%22%3A%22text%2Fcss%22%2C%22ext%22%3A%22css%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22nozIE%22%2C%22card%22%3A%22file%22%7D)
#### 4.3. vue.config.js和.editorconfig
配置别名，这样就可以避免../../等写法。vue.config.js可以配置。
webpack已经默认配置了1个别名：@:"src"
先在根目录下新建vue.config.js，然后写入以下配置：
![](https://s2.loli.net/2022/07/12/VJ2MQHwAF7cxOri.png)
新建.editorconfig，用于配置基本的编程规范，如缩进，换行等。
![](https://s2.loli.net/2022/07/12/qf5y4Sb7IgQCaiD.png)
#### 4.4. 项目的模块划分: tabbar -> 路由映射关系

#### 4.5. 首页开发

- navbar 的封装
- 网络数据的请求
- 轮播图
- 推荐信息

```javascript
git remote add origin https://github.com/coderwhy/testmall.git
git push -u origin master
```

sync -> 同步

async -> 异步

aysnc operation: 操作

xcode/iphonex/xml

token ->

linus -> linux/git
