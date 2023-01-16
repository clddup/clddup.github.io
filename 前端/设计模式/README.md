## JS 设计模式

### 一.设计模式介绍

- 设计模式是我们在解决问题的时候针对特定问题给出的简介而优化的处理方案
- 在 JS 设计模式中, 最核心的思想: 封装变化
- 将变与不变分离, 确保变化的部分灵活, 不变的部分稳定

### 二. 构造器模式

```javascript
var employee1 = {
  name: "cl-ddup",
  age: 100,
};
var employee2 = {
  name: "clddup",
  age: 200,
};
```

以上写法, 如果数据量变多, 代码重复且臃肿

```javascript
function Employee(name, age) {
  this.name = name;
  this.age = age;
}

Employee.prototype.say = function () {
  console.log(`hello ${this.name} - ${this.age}`);
};

var employee1 = new Employee("cl-ddup", 100);
var employee2 = new Employee("clddup", 200);

employee1.say();
employee2.say();
```

### 三.原型模式

```javascript
class Employee {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  say() {
    console.log(`hello ${this.name} - ${this.age}`);
  }
}

var employee1 = new Employee("cl-ddup", 100);
var employee2 = new Employee("clddup", 200);

employee1.say();
employee2.say();
```

### 四.工厂模式

> 由一个工厂对象决定创建某一种产品对象类的实例, 主要用来创建同一类对象

```javascript
class User {
  constructor(role, pages) {
    this.role = role;
    this.pages = pages;
  }

  static UserFactory(role) {
    switch (role) {
      case "superadmin":
        return new this("superadmin", [
          "home",
          "user-mange",
          "right-mange",
          "new-mange",
        ]);
        break;
      case "admin":
        return new this("admin", ["home", "user-mange", "new-mange"]);
        break;
      case "editor":
        return new this("admin", ["home", "new-mange"]);
        break;
      default:
        throw new Error("参数错误");
    }
  }
}

User.UserFactory("superadmin");
User.UserFactory("admin");
User.UserFactory("editor");
```

简单工厂的有点在于, 你只需要一个正确的参数, 就可以获取到你所需要的对象, 而无需知道其创建的具体细节. 但是在函数内包含了所有对象的创建逻辑和判断逻辑的代码, 每增加新的构造函数还需要修改判断逻辑代码. 当我们的对象不是上面的 3 个而是 10 多个或更多时, 这个函数会成为一个庞大的超级函数, 难以得到维护. 所以, 简单工厂只能作用于**创建的对象数量较少, 对象的创建逻辑不复杂时使用**.

### 五.抽象工厂模式

> 抽象工厂模式并不直接生成实例, 而是对于产品类簇的创建

```javascript
class User {
  constructor(name, role, pages) {
    this.name = name;
    this.role = role;
    this.pages = pages;
  }

  welcome() {
    console.log("欢迎回来", this.name);
  }

  dataShow() {
    throw new Error("抽象方法不允许直接调用");
  }
}

class SuperAdmin extends User {
  constructor(name) {
    super(name, "superadmin", ["home", "user-mange", "right-mange", "new-mange"]);
  }

  dataShow(){
    console.log("superadmin-datashow")
  }

  addRight(){

  }

  addUser(){

  }
}
class Admin extends User {
  constructor(name) {
    super(name, "admin", ["home", "user-mange", "new-mange"]);
  }

  dataShow(){
    console.log("admin-datashow")
  }

  addRight(){

  }

  addUser(){
    
  }
}
class Editor extends User {
  constructor(name) {
    super(name, "editor", ["home", "new-mange"]);
  }

  dataShow(){
    console.log("editor-datashow")
  }
}

function getAbstractUserFactory(role){
  switch(role){
    case "superadmin":
      return SuperAdmin
    case "admin":
      return Admin
    case "editor":
      return  Editor
  }
}

let userClass = getAbstractUserFactory('superadmin')
let user = new userClass("cl-ddup")
user.welcome()
```

### 六. 建造者模式
> 建造者模式(builder pattern)属于创建型模式的一种, 提供一种创建复杂对象的方式. 它将一个复杂的对象的构建与它的表示分离, 使同样的构建过程可以创建不同的表示.

>建造者模式是一步一步的创建一个复杂的对象, 它允许用户只通过指定复杂的对象的类型和内容就可以构建它们, 用户不需要指定内部的具体构造细节.

```javascript
class Navbar {
  init(){
    console.log('navbar-init')
  }

  getData(){
    return new Promise(resolve => {
      setTimeout(()=>{
        resolve()
        console.log('navbar-getData')
      },1000)
    })
  }

  render(){
    console.log('navbar-render')
  }
}

class List {
  init(){
    console.log('list-init')
  }
  getData(){
    return new Promise(resolve => {
      setTimeout(()=>{
        resolve()
        console.log('list-getData')
      },1000)
    })
  }
  render(){
    console.log('list-render')
  }
}

class Creator {
  async startBuild(builder){
    await builder.init()
    await builder.getData()
    await builder.render()
  }
}

const op = new Creator()
op.startBuild(new Navbar())
op.startBuild(new List())
```
>建造者模式就是将一个复杂对象的构建与其表示层互相分离, 同样的构建过程可采用不同的表示. 工厂模式主要是为了创建对象实例或者类簇 (抽象工厂), 关心的是最终产出(创建)的是什么, 而不关心创建的过程. 而建造者模式关心的是创建这个对象的过程, 甚至于创建对象的每一个细节.

### 七.单例模式
- 保证一个类仅有一个实例, 并提供一个访问它的全局访问点
- 主要解决一个全局使用的类频繁的创建和销毁, 占用内存

```javascript
class Singleton {
  constructor(name, age){
    if(!Singleton.instance){
      this.name = name
      this.age = age
      Singleton.instance = this
    }

    return Singleton.instance
  }
}

new Singleton('clddup', 18) == new Singleton('cl-ddup', 18)
```

### 八.装饰器模式
> 装饰器模式能够很好的对已有功能进行拓展, 这样不会更改原有代码, 对其他的业务产生影响, 这方便我们在好少的改动下对软件进行拓展

```javascript
Function.prototype.before = function(beforeFn){
  var _this = this
  return function(){
    // 先进行前置函数调用
    beforeFn.apply(this, arguments)
    // 执行原来的函数
    return _this.apply(this, arguments)
  }
}

Function.prototype.after = function(afterFn){
  var _this = this
  return function(){
    // 先执行原来的函数
    var ret = _this.apply(this, arguments);
    // 在执行后置函数调用
    afterFn.apply(this, arguments)
    return ret
  }
}

function test(){
  console.log('11111')
}

var test1 = test.before(()=>{
  console.log('00000')
}).after(()=>{
  console.log('22222')
})

test1()
```

### 九.适配模式
> 将一个类的接口转换成客户希望的另一个接口, 适配器模式让那些接口不兼容的类可以一起工作

```javascript
// 按照官网代码复制
class TencentMap {
  show(){
    console.log('开始渲染腾讯地图')
  }
}

// 按照官网代码复制
class BaiduMap {
  display(){
    console.log('开始渲染百度地图')
  }
}

class TencentAdapater extends TencentMap {
  constructor(){
    super()
  }
  display(){
    this.show()
  }
}

function renderMap(map){
  map.display()

}

renderMap(new TencentAdapater())
renderMap(new BaiduMap())


```

### 十.策略模式
> 策略模式定义了一些列算法, 并将每个算法封装起来, 使它们可以互相转换, 且算法的变化不会影响算法的客户, 策略模式属于对象行为模式, 它通过对算法进行封装, 把使用算法的责任和短发的实现分割开来,并委派给不同的对象对这些算法进行管理

>该模式**主要解决**在有多种算法想死情况下吗使用**if...else**所带来的复杂和难以维护. 它的**优点**是算法可以自由切换, 同时可以避免多重**if...else**判断, 且具有良好的扩展性

```javascript
let strategry = {
  "S": (salary)=>{
    return salary*6
  },
  "A": (salary)=>{
    return salary*4
  },
  "B": (salary)=>{
    return salary*3
  },
  "C": (salary)=>{
    return salary*2
  }
}

function calBonus(level, salary){
  return strategry[level](salary)
}

console.log(calBonus('A', 10000))
console.log(calBonus('B', 8000))
console.log(calBonus('C', 6000))

```

### 十一.代理模式
> 代理模式 (Proxy), 为其他对象提供一种代理以控制对这个对象的访问

>代理模式使得代理对象控制裤头对象的引用, 代理几乎可以是任何对象: 文件, 资源, 内存中对象, 或者是一些难以复制的东西

```javascript
var star = {
  name: "tiechui",
  workprice: 10000
}

let proxy = new Proxy(star, {
  get(target, key){
    if(key == 'workprice'){
      console.log('访问了')
    }
    return target[key]
  },
  set(target, key, value){
    if(key === 'workprice'){
      console.log('设置了');
      if(value > 10000){
        console.log('可以合作')
      }else{
        throw new Error("价钱不合适")
      }
    }
  }
})

console.log(proxy.workprice)
proxy.workprice = 20000
proxy.workprice = 2000
```
### 十二.观察者模式
> 观察者模式包含观察目标和观察者两类对象

> 一个目标可以有任意数目的与之相依赖的观察者

> 一旦观察目标的状态发生变化, 所有观察者都将得到通知

当一个对象的状态发生改变时, 所有依赖于它的对象都得到通知并被自动更新, 解决了主体对象与观察者之间功能的耦合, 即一个对象状态改变给其他对象通知问题

```javascript
class Subject {
  constructor(){
    this.observers = []
  }
  add(observer){
    this.observers.push(observer)
  }
  notify(){
    console.log(this);
    this.observers.forEach(item => {
      item.update()
    })
  }
  remove(observer){
    // splice
    // filter
    this.observers = this.observers.filter(item => item != observer)
    console.log(this.observers);
  }
}

class Observer {
  constructor(name){
    this.name = name
  }
  update(){
    console.log(this.name, 'update');
  }
}

const subject = new Subject()
const observer1 = new Observer('clddup')
const observer2 = new Observer('cl-ddup')

subject.add(observer1)
subject.add(observer2)

setTimeout(()=>{
  subject.notify()
}, 2000)
setTimeout(()=>{
  subject.remove(observer1)
}, 1000)
```
优势: 目标者与观察者, 功能耦合度较低, 专注自身功能逻辑; 观察者被动接收更新, 时间上解耦, 实际接收目标者更新状态

缺点: 观察者模式虽然实现了对象间依赖关系的低耦合, 但却不能对事件通知进行细分管控, 如"筛选通知", "指定主题事件通知"

### 十三.发布订阅模式
> 1.观察者和目标要相互知道

> 2.发布者和订阅者不用互相知道, 通过第三方实现调度, 属于经过解耦合的观察者模式

```javascript
// publish 发布
// subscribe 订阅 
const PubSub = {
  message: {},
  publish(type, data){
    if(!this.message[type]) return 
    this.message[type].forEach(item => item(data))
  },
  subscribe(type, cb){
    if(!this.message[type]){
      this.message[type] = [cb]
    }else{
      this.message[type].push(cb)
    }
    
  },
  unsubscribe(type, cb){
    if(!this.message[type]) return 
    if(!cb){
      // 取消所有当前type事件
      this.message[type] && (this.message[type].length = 0)
    }else{
      this.message[type] = this.message[type].filter(item => item != cb)
    }
  }
}


function testA(data){
  console.log("testA", data)
}

function testB(data){
  console.log("testB", data)
}

PubSub.subscribe("A", testA)
PubSub.subscribe("B", testB)

PubSub.publish('A', 111)
PubSub.publish('B', 222)

PubSub.unsubscribe('A', testA)

PubSub.publish('A', 111)
PubSub.publish('B', 222)

```

### 十四.模块模式
> 模块化模式最初被定义为在传统软件工程中为类提供私有和公共封装的一种方法

> 能够使一个单独的对象拥有公共.私有的方法和变量. 从而屏蔽来自全局作用域的特殊部分, 这可以减少我们的函数名与在页面中其他脚本区域内定义的函数名冲突的可能性

1.闭包
```javascript
var obj = (function(){
  var count = 0
  return {
    increment(){
      return ++count
    },
    reset(){
      count = 0
    },
    decrement(){
      return --count
    }
  }
})()

console.log(obj.increment())
console.log(obj.increment())
console.log(obj.increment())
console.log(obj.reset())
console.log(obj.decrement())
console.log(obj.decrement())
console.log(obj.decrement())

```

2.模块化
```javascript
// 1.js 内容
let count = 0
function increase(){
  return ++count
}

function decrease(){
  return --count
}
export default {
  increase,
  decrease
}
```
<script type="module">

</script>
```javascript
<script type="module">
  import myObj from './1.js'
  console.log(myObj.increase())
  console.log(myObj.decrease())
</script>
```
