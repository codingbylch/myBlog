---
title: "vue(1) 基础_组件化_模块化"
date: 2021-03-25T00:00:00+08:00
draft: false
tags: ['Vue', 'coderwhy', '基础']
categories: ['前端']
---

### 一. 邂逅Vuejs

#### 1.1. 认识Vuejs

- 为什么学习Vuejs
- Vue的读音
- Vue的渐进式
- Vue的特点
   - 解耦视图和数据
   - 可复用的组件
   - 前端路由技术
   - 状态管理
   - 虚拟DOM

#### 1.2. 安装Vue

- CDN引入
> <!-- 开发环境版本，包含了有帮助的命令行警告 -->  
> <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
> <!-- 生产环境版本，优化了尺寸和速度 -->
> <script src="https://cdn.jsdelivr.net/npm/vue"></script>

- 下载引入
> 开发环境 [https://vuejs.org/js/vue.js](https://vuejs.org/js/vue.js)  
> 生产环境 [https://vuejs.org/js/vue.min.js](https://vuejs.org/js/vue.min.js)

- npm安装
> 通过webpack和CLI的使用


#### 1.3. Vue的初体验
#### 1.4. Vue中的MVVM
M：Model，数据层
V：View，视图层，面向用户展示信息
VM：VueModel，视图模型层，View和Model层沟通的桥梁，实现数据绑定、数据监听。即Model、View一方发生改变，另一方自动变化
#### 1.5. 创建Vue时, options可以放那些东西

- el: string | HTMLElement，决定之后Vue实例会管理哪一个DOM
- data: Object | Function，Vue实例对应的数据对象
- methods:  { [key: string]: Function }，定义属于Vue的一些方法
- 生命周期函数

![](https://s2.loli.net/2022/07/12/ZrRGH1WMwLfVdac.png)
![](https://s2.loli.net/2022/07/12/mJrBsRLnWU1Iftl.png)
### 二.插值语法

- mustache语法：展示data中的数据

`{{message}}`

- v-once：表示元素和组件(组件后面才会学习)只渲染一次，不会随着数据的改变而改变

`<div v-once>{{message}}</div>`

- v-html：按照HTML格式进行解析

`<div v-html='link'></div>`

- v-text：作用和Mustache一致

`<!-- v-text不够灵活 {{}}用的更多 -->`
`<h3 v-text="message"></h3>`

- v-pre: 跳过这个元素和它子元素的编译过程，用于显示原本的Mustache语法

`<h3 v-pre>{{message}}</h3>`

- v-cloak: vue解析前数据不显示, 之后不会用到

`<h3 v-cloak>{{message}}</h3>`
### 三. v-bind
#### 3.1. v-bind绑定基本属性href/src

- v-bind:src
- :href 简写
#### 3.2. v-bind动态绑定class

- 对象语法: 作业 :class='{类名: boolean}'

`<h2 class="title" :class="{'active': isActive, 'line': isLine}">Hello World</h2>`

- 数组语法:

`<h2 class="title" :class="['active’,'line']">Hello World</h2>`
#### 3.3. v-bind动态绑定style

- 对象语法:

`:style="{color: currentColor, fontSize: fontSize + 'px'}"`

- 数组语法:

`<div v-bind:style="[baseStyles, overridingStyles]"></div>`
### 一. 计算属性
#### 1.1. 计算属性的本质

- fullname: {set(), get()}

#### 1.2. 计算属性和methods对比

- 计算属性在多次使用时, 只会调用一次.
- 它是有缓存的

- 案例一: firstName+lastName
```javascript
//   计算属性computed，与methods的区别：有缓存，不必每次重新计算
computed: {
  fullName: function () {
    // 这是一种简写方式, get方法
    return this.message + " " + this.obj.name;
  },
    // fullName:{
    //     get:function(){
    //         return this.message +" "+ this.obj.name;
    //     },
    //     set:function(newValue){
    //         // console.log(newValue)
    //     }
    // }
}
```
### 
### 二. 事件监听v-on
简写：@
#### 2.1. 事件监听基本使用
#### 2.2. 参数问题

- btnClick
- btnClick(event)：默认event参数
- btnClick(abc, **$event**)：传参注意写法
```html
<div>
    <!-- 事件监听v-on，传参问题 -->
    <p @click="btnClick">v-on传参:MouseEvent</p>
    <p @click="btnClick()">v-on传参:undefined</p>
    <p @click="btnClick(123)">v-on传参:123</p>
    <!-- 需要参数、又需要event对象的写法 -->
    <p @click="btnClickAnother(123,$event)">v-on传参:123+event对象</p>
</div>
```
#### 2.3. 修饰符

- .stop：阻止冒泡
- .prevent：阻止默认事件
- .enter：监听enter按键
- .once：只监听一次
- .native：使自定义组件可响应原生事件，不局限于自定义事件
```html
<div>
    <!-- v-on修饰符 -->
    <!-- .stop，阻止内到外冒泡 -->
    <div @click="outClick">
      <button @click.stop="inClick">按钮</button>
    </div>

    <!-- .prevent阻止默认事件 -->
    <form action="baidu">
      <input type="submit" value="提交" @click.prevent="submitBtn" />
    </form>

    <!-- 监听键盘某个按键点击，这里监听enter建 -->
    <input type="text" @keyup.enter="keyUp" />

    <!-- .once 只监听一次，用户第一次点击才有效 -->
    <button @click.once="inClick">按键</button>

    <!-- .native 使自定义组件能够使用原生事件, 如@click.native -->
</div>
```
### 三. 条件判断
#### 3.1. v-if/v-else-if/v-else
#### 3.2. 登录小案例
```html
<div>
  <!-- v-if、v-else-if和v-else的使用 -->
  <div v-if="isShow">v-if的使用</div>
  <div v-else>v-else的使用</div>
  <button @click="vifBtn">控制v-if</button>

  <div>
    <div v-if="isUser">
      <span for="username">用户账号</span>
      <input
             type="text"
             id="username"
             placeholder="请输入用户名"
             key="username"
             />
    </div>
    <div v-else>
      <span for="useremail">用户邮箱</span>
      <input
             type="text"
             id="useremail"
             placeholder="请输入用户邮箱"
             key="email"
             />
    </div>
    <button @click="switchBtn">切换</button>
  </div>
</div>
```
#### 3.3. v-show

- v-show和v-if区别

两者消失的方式不同，选择看使用频率：
v-if条件为false时, 元素不存在于DOM中
 v-show, 样式增加 display:none，控制台可查看到对应代码
### 四. 循环遍历
#### 4.1. 遍历数组
写法：
`<span v-for='item in arr'>{{item}}</span>`
`<span v-for='(item,index) in arr'>{{item}}</span>`
#### 4.2. 遍历对象
写法：

- value
- value, key
- value, key, index

#### 4.3. 数组哪些方法是响应式的
响应式方法：push、pop、shift、unshfit、splice：删除、插入、替换
另一种方案,替换：Vue.set(obj,start,target)
arr[xx]=yy 不是响应式
### 五. 书籍案例
略。
### 六. v-model的使用：表单数据双向绑定
#### 6.1. v-model的基本使用
本质是语法糖：

- v-model => v-bind:value + v-on:input

`<input type="text" v-model="message" />` 等价于：
```html
<input type="text" :value="message" @input="valueChange" />
valueChange(e){
	this.message = e.target.value
}
```
#### 6.2. v-model和radio/checkbox/select
```html
<!-- 结合radio类型 -->
<label for="male">
  <input type="radio" value="男" v-model="sex" />男
</label>
<label for="female">
  <input type="radio" value="女" v-model="sex" />女
</label>

<!-- 结合checkbox类型 -->
<label for="agree">
  <input type="checkbox" id="agree" v-model="isAgree" />同意协议
</label>

<!-- checkbox多选框 -->
<label :for="item" v-for="item in hobbiesArr">
  <input
         type="checkbox"
         v-model="hobbies"
         :value="item"
         :id="item"
         />{{item}}
</label>

<!-- 结合select类型使用 -->
<select name="" id="" v-model="selectFruit">
  <option value="苹果">苹果</option>
  <option value="香蕉">香蕉</option>
  <option value="梨子">梨子</option>
  <option value="火龙果">火龙果</option>
</select>
```
#### 6.3. 修饰符

- lazy：懒加载
- number：只能输入数字
- trim：去除输入两边空格
```html
<!-- .lazy 懒加载 -->
<input type="text" v-model.lazy="message" />
<!-- .number 只能输入数字 -->
<input type="text" v-model.number="age" />{{age}} {{typeof(age)}}
<!-- .trim 去除输入的两边空格-->
<input type="text" v-model.trim="name" />
```
### 七. 组件化开发
#### 7.1. 认识组件化
![](https://s2.loli.net/2022/07/12/V514stxYAowS2rv.png)
#### 7.2. 组件的基本使用
组件的使用分成三个步骤（之前）：

- 创建组件构造器：Vue.extend({...})的形式（已很少使用）
- 注册组件：Vue.component
- 使用组件

现在一般这样进行注册：
```html
<template id="mycpn">
  <div>分离式写法</div>
</template>
<script>
  const cpn = {
    template:'#mycpn',
    data(){return{}},
  }
  const app = new Vue({
  	el:'#app',
    components:{
      cpn, // 局部注册，只能在app实例中使用，vue实例相当于父组件, cpn是子组件
    }
  })
</script>
```
#### 7.3. 全局组件和局部组件
局部组件注册如上，全局组件如下：
```html
<template id="mycpn">
  <div>分离式写法</div>
</template>
<script>
  const cpn = {
    template:'#mycpn',
    data(){return{}},
  }
  Vue.component('cpn1',cpn) // 全局注册可在任一vue实例使用
</script>
```
#### 7.4. 父组件和子组件
如上
#### 7.5. 注册的语法糖
如上，Vue.component('cpn',{...})，这里的对象{...}，vue内部中就包含了Vue.extend的相关操作
#### 7.6. 模板的分类写法

- script：`<script type='text/x-template' id='cpn'></script>`
- template：`<template id='cpn'></template>`

如上例子
#### 7.7. 数据的存放

- 子组件不能直接访问父组件
- 子组件中有自己的data, 而且必须是一个函数.
- 为什么必须是一个函数.
> 首先若不是，vue会报错；其次，原因在于每次让组件对象都返回一个新的对象，return {...}

#### 7.8. 父子组件的通信
[父子组件通信.html](https://www.yuque.com/attachments/yuque/0/2021/html/714353/1614499803517-a5986550-a2ff-4761-8b7a-2c74567c81c7.html?_lake_card=%7B%22uid%22%3A%221614499802979-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fhtml%2F714353%2F1614499803517-a5986550-a2ff-4761-8b7a-2c74567c81c7.html%22%2C%22name%22%3A%22%E7%88%B6%E5%AD%90%E7%BB%84%E4%BB%B6%E9%80%9A%E4%BF%A1.html%22%2C%22size%22%3A2712%2C%22type%22%3A%22text%2Fhtml%22%2C%22ext%22%3A%22html%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22q469w%22%2C%22card%22%3A%22file%22%7D)
[父子通信结合双向绑定.html](https://www.yuque.com/attachments/yuque/0/2021/html/714353/1614502178393-67a4850f-92f4-4826-b681-6b864d8dab88.html?_lake_card=%7B%22uid%22%3A%221614502178398-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fhtml%2F714353%2F1614502178393-67a4850f-92f4-4826-b681-6b864d8dab88.html%22%2C%22name%22%3A%22%E7%88%B6%E5%AD%90%E9%80%9A%E4%BF%A1%E7%BB%93%E5%90%88%E5%8F%8C%E5%90%91%E7%BB%91%E5%AE%9A.html%22%2C%22size%22%3A1630%2C%22type%22%3A%22text%2Fhtml%22%2C%22ext%22%3A%22html%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%222Ishr%22%2C%22card%22%3A%22file%22%7D)

- 父传子: props

基本用法：props可以是数组/对象，若为对象，可设置默认值、数据验证
```javascript
// 在子组件中有个属性props，可以是数组/对象
props: {
  	cmessage: String,
    cHobby: {
      type: Array,
        default() {
          // 引用类型，需返回函数
          return ["篮球"];
        },
   	  required: true,
    },
},
```

- 子传父: $emit，自定义事件
```javascript
methods: {
  itemClick(id) {
    console.log(id);
    // 子组件发射事件, 参数：事件名，参数
    this.$emit("xxx", id);
  },
},
```
在vue2.3.0中新增了.sync修饰符，用于简化子组件传值给父组件的方式：
```html
<comp :foo.sync="bar"></comp>
// 会被扩展为
<comp :foo="bar" @update:foo="val => bar = val"></comp>

// 当子组件需要更新foo的值时，需要显式地触发一个更新事件：
this.$emit('update:foo', newValue)
```
### 一. 组件化开发
[访问子组件对象$children、$refs.html](https://www.yuque.com/attachments/yuque/0/2021/html/714353/1614502203603-a0f7dd48-6f42-4a74-940e-8f1408516531.html?_lake_card=%7B%22uid%22%3A%221614502203802-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fhtml%2F714353%2F1614502203603-a0f7dd48-6f42-4a74-940e-8f1408516531.html%22%2C%22name%22%3A%22%E8%AE%BF%E9%97%AE%E5%AD%90%E7%BB%84%E4%BB%B6%E5%AF%B9%E8%B1%A1%24children%E3%80%81%24refs.html%22%2C%22size%22%3A1002%2C%22type%22%3A%22text%2Fhtml%22%2C%22ext%22%3A%22html%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22YSo24%22%2C%22card%22%3A%22file%22%7D)
#### 1.1. 父子组件的访问

- 父得到子组件对象：$children（少用）/ $refs（多用）
> vue实例有属性$children，表示在实例中注册的子组件对象数组，通过下标访问某个具体子组件对象，因此很少用。$refs用的多，在父子组件中对子组件进行ref标记，如ref='aaa'，父组件中可通过this.$refs.aaa得到子组件对象。

```html
<div id='app'>
	<cpn ref="aaa"></cpn>
</div>
```

- 子得到父组件对象：$parent（少用）/$root（获取根组件对象）
> 开发中不建议使用$parent，因为考虑子组件的复用性，其父组件对象不固定的。造成耦合度很高，不利于复用。

#### 
#### 1.2. slot插槽的使用
[slot.html](https://www.yuque.com/attachments/yuque/0/2021/html/714353/1614502213392-5491f0fd-8407-4f86-9132-9b970ce04aef.html?_lake_card=%7B%22uid%22%3A%221614502213535-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fhtml%2F714353%2F1614502213392-5491f0fd-8407-4f86-9132-9b970ce04aef.html%22%2C%22name%22%3A%22slot.html%22%2C%22size%22%3A1917%2C%22type%22%3A%22text%2Fhtml%22%2C%22ext%22%3A%22html%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22wKImJ%22%2C%22card%22%3A%22file%22%7D)
slot让组件具有扩展性，预留空间，让父子组件中使用时，有更多自定义性。
抽取共同部分，将不同处作为插槽。

- 基本使用

`<slot><button>插槽默认值</button> </slot>`

- 具名插槽：多个插槽时进行区分

vue2.6.0+废弃了父组件中slot的写法，`slot='right' --> v-slot:right`，注意 **`v-slot` 只能添加在 `<template>` 上** (只有[一种例外情况](https://cn.vuejs.org/v2/guide/components-slots.html#独占默认插槽的缩写语法))，v-slot可缩写为`v-slot:right --> #right`
```html
// 子组件：
<slot name="left"><button>左边</button></slot>
<slot name="middle"><button>中间</button></slot>
<slot name="right"><button>右边</button></slot>

// 父组件
<cpn2>
  <span slot="middle">具名插槽</span> // 已废弃
  <template v-slot:middle>
    <span>具名插槽</span>
  </template>
</cpn2>
```

- 编译的作用域概念
> 父组件模板的所有东西都会在父级作用域内编译；子组件模板的所有东西都会在子级作用域内编译。

- 作用域插槽：父组件决定子组件插槽标签展示形式，子组件决定插槽数据

vue2.6.0+版本：`slot='dedault' slot-scope='slot' --> v-slot:default='slot'`，多个插槽: v-slot:插槽名='数据调用方法名'，可缩写为：`v-slot:default='slot' --> #default='slot'`
```html
// 子组件
<template id="cpn3">
  <div>
    <div>作用域插槽:</div>
    <slot :mydata="language">
      <ul>
        <li v-for="item in language">{{item}}</li>
      </ul>
    </slot>
  </div>
</template>

// 父组件
<div>
  <cpn3>
    // 2.6.0+使用v-slot
    <template v-slot:default="slot">
      <span>{{slot.mydata.join(' - ')}}</span>
    </template>
  </cpn3>
  <cpn3></cpn3>
</div>
```
[独占默认插槽的缩写语法](https://cn.vuejs.org/v2/guide/components-slots.html#独占默认插槽的缩写语法)
解构插槽Prop
动态插槽名：2.6.0新增
具名插槽的缩写：2.6.0新增
### 二. 前端模块化
#### 2.1. 为什么要使用模块化

- 简单写js代码带来的问题
> JS文件过多时，整理顺序麻烦；多人开发命名冲突等；

- 立即执行匿名函数解决命名问题，但代码不可复用
- 自己实现了简单的模块化：在匿名函数中定义对象return出去
- AMD/CMD/CommonJS/ES6 Modules语法：
> 其中CommonJS在node环境中使用，ES6 Modules在平时开发时使用

CommonJS导入导出：
```javascript
//A.js
function add(a,b){
  return a+b;
}
let obj = {name:'vue'}
// 导出
module.exports = {add, obj} // 用了对象简写形式(es6)

// B.js
let {add} = require('./A.js')
add(5,6);
```
#### 2.2. ES6中模块化的使用

- export 导出
- import 导入
```javascript
// A.js
let a = 1;
let b = 2;
let obj = {}
export function add(c,d){return c+d}
export {a,b}
export default obj // 只能存在1个

// B.js
import {a,b} from './A.js' 
// or
import myobj from './A.js' // 为默认
// or
import * from './A.js' // 导入全部
```
### 
