---
title: "Clean Code For JS 指南"
date: 2022-07-07T00:23:56+08:00
draft: false
---

仅供参考，不必可以遵循。
### 变量
变量名称有意义、可读；
使用可搜索的变量名，如数字常量可用const大写变量定义；
使用解释性的变量，用上数组解构、对象解构；
```javascript
const [, city, zipCode] = address.match(cityZipCodeRegex) || [];
```
如果类名/对象名有意义，则其子属性变量名不要重复：good：`car.color = 'Red';`
使用默认变量的形式：
```javascript
// bad 
function createMicrobrewery(name) {
  const breweryName = name || 'Hipster Brew Co.';
  // ...
}
// good
function createMicrobrewery(breweryName = 'Hipster Brew Co.') {
  // ...
}
```
### 函数
**函数只做一件事：**
```javascript
// not good
function emailClients(clients) {
  clients.forEach((client) => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
// good
function emailClients(clients) {
  clients
    .filter(isClientActive)
    .forEach(email);
}

function isClientActive(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```
**函数参数<=2个最好，3个及以上就使用对象形式；**
```javascript
// good
const menuConfig = {
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
};
function createMenu(config) {
  // ...
}
```
**函数名要有意义，从函数名能知道这个函数做什么；**
函数应该只有一个抽象级别，拆分可提升重用性、测试性；
**尽量移除冗余代码**，即只需创建一个函数/模块/类，就能在多处处理，同时这也要求有一定的抽象能力；
```javascript
function showDeveloperList(developers) {
  developers.forEach((developer) => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();
    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers) {
  managers.forEach((manager) => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
// good
function showList(employees) {
  employees.forEach((employee) => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    let portfolio = employee.getGithubLink();

    if (employee.type === 'manager') {
      portfolio = employee.getMBAProjects();
    }

    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```
**使用Object.assign设置默认对象；**
```javascript
function createMenu(config) {
  config = Object.assign({
    title: 'Foo',
    body: 'Bar',
    buttonText: 'Baz',
    cancellable: true
  }, config);
```
不要用标记为做函数参数：
```javascript
// bad 
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
// good
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```
**避免副作用**，比如函数尽量不产生副作用；比如写入文件，用一个服务去处理而不是好多个函数都可以；
不要写入全局函数。如不要在JS原生Array上直接添加方法，可能会覆盖原有方法；更好的做法是使用ES6的extends；
```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```
**尽量使用函数式编程风格进行编程；**
封装条件语句（仅参考）；
避免负面条件；
避免条件语句（仅参考），这条我感觉不行；
避免类型检查，请使用TS；
不要过度优化，浏览器已经背后做了：在旧的浏览器上， 每次循环 `list.length` 都没有被缓存， 会导致不必要的开销，现代化浏览器已经优化这个；
移除僵尸（无用）代码；

### 对象
使用get和set访问、修改对象上的属性；（待评估）
```javascript
class BankAccount {
  constructor(balance = 1000) {
    this._balance = balance;
  }

  // It doesn't have to be prefixed with `get` or `set` to be a getter/setter
  set balance(amount) {
    if (verifyIfAmountCanBeSetted(amount)) {
      this._balance = amount;
    }
  }

  get balance() {
    return this._balance;
  }

  verifyIfAmountCanBeSetted(val) {
    // ...
  }
}

const bankAccount = new BankAccount();

// Buy shoes...
bankAccount.balance -= shoesPrice;

// Get balance
let balance = bankAccount.balance;
```
让对象拥有私有成员可通过闭包来实现；
优先使用ES6语法所提供的类class和继承extends，代替ES5的语法；
**使用方法链（可以试试）**，只要在类里的方法中的最后返回this即可连在一起：
```javascript
class Car {
  constructor() {
    this.make = 'Honda';
    this.model = 'Accord';
    this.color = 'white';
  }

  setMake(make) {
    this.make = make;
    // NOTE: Returning this for chaining，即返回类的实例对象
    return this;
  }

  setModel(model) {
    this.model = model;
    // NOTE: Returning this for chaining
    return this;
  }

  setColor(color) {
    this.color = color;
    // NOTE: Returning this for chaining
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    // NOTE: Returning this for chaining
    return this;
  }
}

const car = new Car()
  .setColor('pink')
  .setMake('Ford')
  .setModel('F-150')
  .save();

```

### 原则
开闭原则OCP：允许用户添加功能而不必修改现有代码；
单一职责原则SRP；
里氏代换原则LSP；
接口隔离原则ISP；
依赖反转原则DIP；


测试：为新功能/模块编写测试；或测试驱动开发；
### 错误处理
不要忽略捕捉的错误：
```javascript
// bad
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
// good
try {
  functionThatMightThrow();
} catch (error) {
  // One option (more noisy than console.log):
  console.error(error);
  // Another option:
  notifyUserOfError(error);
  // Another option:
  reportErrorToService(error);
  // OR do all three!
}
对于Promise中的catch也是相同处理
```
### 格式化
格式化是主观的，不要浪费时间去争论，团队有就用团队的。
函数的调用方与被调用方应该靠近；

### 注释
**仅仅对包含复杂业务逻辑的东西进行注释；**
```javascript

// good
function hashIt(data) {
  let hash = 0;
  const length = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    hash = ((hash << 5) - hash) + char;

    // Convert to 32-bit integer
    hash &= hash;
  }
}
```
不要在代码库中保存注释掉的代码，因为有版本控制；
不要有日志式的注释，使用版本控制即可；
避免占位符；




