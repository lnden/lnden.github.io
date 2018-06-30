---
title: ECMAScript
date: 2018-02-09 17:21:23
tags:
---
### ECMAScript6

#### 对象

class类
1.类里面有一个检查子类是否是继承的父类

```
class A {}

calss B extends A {
    constructor(){
        super()
    }
}

Object.getPrototypeOf(B) === A     //为true

使用Object.getPrototypeOf检查
```

#### 声明变量的六种方法

```
var

function

let 

const 

class

import 

```
#### 模板字符串

```
var name = 'lily';
console.log('hello' + name);

var name = 'lucy';
console.log(`hello ${name}`)
```
#### 函数
es5
```
function action(num){
    num = num || 200
    return num;
}
```
es6
```
function action(num = 200){
    console.log(num);
}
action();       //200
action(300);    //300
```

#### Object.keys()

Object.keys方法是JavaScript中用于遍历对象属性的一个方法。它传入的参数是一个对象，返回的是一个数组，数组中包含的是该对象所有的属性名。
```
1.讲对象转换为数组，类似set数据类型
let object = {
    name:'lily',
    age:'25',
    sex:'woman',
    address:'America',
    occupation:'webFE(front-endX)'
}
let newArr = Object.keys(object);
console.log(newArr); // console: ["name", "age", "address", "city"]

2.将对象转换成为数组，随机键排序
var an_obj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.keys(an_obj)); // console: ['2', '7', '100']
```

#### Set和Map数据结构

#### 属性的简洁表示
对象，函数都可以简写
```
var birth = '2000/01/01';
var Person = {
  name: '张三',
  //等同于birth: birth
  birth,
  // 等同于hello: function ()...
  hello() { console.log('我的名字是', this.name); }
};
```
#### 合并对象 === $.extend({},{})
```
Object.assign()
var target = { a: 1 };

var source1 = { b: 2 };
var source2 = { c: 3 };

Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}

还可以使用  const c = {...a,...b}  //进行合并
```

#### 扩展运算符
```
let z = {a:3,b:4};
let n = {...z};
console.log(n);
```
    
#### 判断是否包含对应值
```
// 1.includes：判断是否包含然后直接返回布尔值
let str = 'hahay'
console.log(str.includes('y')) // true

```

#### 重复让值显示
```
// 2.repeat: 获取字符串重复n次
let s = 'he'
console.log(s.repeat(3)) // 'hehehe'
//如果你带入小数, Math.floor(num) 来处理
```

#### 函数默认参数问题
```
function action(num) {
    num = num || 200
    //当传入num时，num为传入的值
    //当没传入参数时，num即有了默认值200
    return num
}
    //传0的时候还是默认为false 
```
```
    function action(num = 200) {
        console.log(num)
    }
    action() //200
    action(300) //300
```

#### 箭头函数

```
    var people = name => 'hello' + name
    三个特点：
        1.不需要function关键字来创建函数
        2.省略return关键字
        3.继承当前上下文的 this 关键字
```

#### 数据访问--解构
es5
```
const people = {
    name: 'lux',
    age: 20
}
const name = people.name
const age = people.age
console.log(name + ' --- ' + age)
```
es6
```
//对象
const people = {
    name: 'lux',
    age: 20
}
const { name, age } = people
console.log(`${name} --- ${age}`)
//数组
const color = ['red', 'blue']
const [first, second] = color
console.log(first) //'red'
console.log(second) //'blue'
```

#### 展开运算符

```
//数组解构
const color = ['red','blue','yellow'];
const colorFull = [...color,'green','pink'];
console.log(color);
console.log(colorFull);
//对象结构
const alp = {
    fist:'a',
    second:'b',
    third:'c'
};
const alphabets = {...alp,four:'d'};
console.log(alphabets);
//数组
const number = [1,2,3,4,5,6,7,8,9];
const [first,second,...rest] = number;
console.log(number);
console.log(rest);
//对象
const user = {
    username:'lux',
    gender:'female',
    age:26,
    address:'peking'
}
const { username, ...rest } = user
console.log(username);
console.log(rest);
```

#### import 和 export

```
1.当用export default people导出时，就用 import people 导入（不带大括号）

2.一个文件里，有且只能有一个export default。但可以有多个export。

3.当用export name 时，就用import { name }导入（记得带上大括号）

4.当一个文件里，既有一个export default people, 又有多个export name 或者 export age时，导入就用 import people, { name, age } 

5.当一个文件里出现n多个 export 导出很多模块，导入时除了一个一个导入，也可以用import * as example

```
#### Promise
&emsp;&emsp; Promise 简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。Promise 的出现，原本是为了解决回调地狱的问题。使得原本的多层级的嵌套代码，变成了链式调用。让代码更清晰，减少嵌套数。
&emsp;&emsp;如果你问我什么是回调地域，那我告诉你就是，一个ajax里面嵌套一个ajax，实现一个需求要嵌套好几层。
&emsp;&emsp;在 Promise 之前代码过多的回调或者嵌套，可读性差、耦合度高、扩展性低。通过 Promise 机制，扁平化的代码机构，大大提高了代码可读性；用同步编程的方式来编写异步代码，保存线性的代码逻辑，极大的降低了代码耦合性而提高了程序的可扩展性。

<strong>1.基本使用</strong>
```
function runAsync(){
    let p = new Promise(function(resolve,reject){
        setTimeout(function(){
            console.log('执行完成');
            resolve('成功的时候执行这一条数据');
        },2000)
    })
    return p;
    
    ||
    
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            console.log('执行完成')
            resolve('使用简单方式执行')
        })
    })
}
runAsync()  //可以执行 '执行完成'
runAsync().then(function(res){   //可以使用then方法进行链式操作
    console.log(res);
})
```
<strong>2.promise.resolve()</strong>
&emsp;&emsp;将现有对象转为 Promise 对象
```
Promise.resolve('foo') // 等价于 new Promise(resolve => resolve('foo'))

1.参数是一个thenable对象
let thenable = {
  then: function(resolve, reject) {
    resolve(42);
  }
};

let p1 = Promise.resolve(thenable);
p1.then(function(value) {
  console.log(value);  // 42
});


2.参数不是具有then方法的对象，或根本就不是对象
const p = Promise.resolve('Hello');

p.then(function (s){
  console.log(s)
});
```


<strong>3.promise.reject()</strong>
&emsp;&emsp; `Promise.reject(reason)` 方法也会返回一个新的 Promise 实例，该实例的状态为 `rejected`。
```
const p = Promise.reject('出错了');
// 等同于
const p = new Promise((resolve, reject) => reject('出错了'))

p.then(null, function (s) {
  console.log(s)
});
// 出错了
```


<strong>4.promise.all()</strong>
&emsp; `all` 表示所有 Promise 对象均有返回状态的时候在执行，如果状态中有一个 `reject` 那就返回第一个执行 `reject` 的状态。
```
1.返回状态全都是resolve
const p1 = new Promise((resolve,reject)=>{
    setTimeout(()=>{
        resolve('等待3秒之后执行')
    },3000)
})

const p2 = new Promise((resolve,reject)=>{
    setTimeout(()=>{
        resolve('等待1秒之后执行')
    },1000)
})
Promise.all([p1,p2]).then(res=>{
    console.log(res);   //返回的是一个数组的p1,p2的集合
    console.log('等待他们都执行成功，你才能看见我');
}).catch(err=>{
    console.log(err)    //错误执行
})


2.如果返回状态有一个是reject
const p1 = new Promise((resolve,reject)=>{
    setTimeout(()=>{
        resolve('等待3秒之后执行')
    },3000)
})

const p2 = new Promise((resolve,reject)=>{
    setTimeout(()=>{
        reject('等待1秒之后执行')
    },1000)
})
Promise.all([p1,p2]).then(res=>{
    console.log(res);   //返回的是一个数组的p1,p2的集合
    console.log('等待他们都执行成功，你才能看见我');
}).catch(err=>{
    console.log(err)    //错误执行 "等待1秒之后执行"
})


3.如果作为参数的 Promise 实例，自己定义了catch方法，那么它一旦被rejected，并不会触发Promise.all()的catch方法。
而是作为参数返回给all里面的catch。
const p1 = new Promise((resolve, reject) => {
  resolve('hello');
})
.then(result => result)
.catch(e => e);

const p2 = new Promise((resolve, reject) => {
  throw new Error('报错了');
})
.then(result => result)
.catch(e => e);

Promise.all([p1, p2])
.then(result => console.log(result))
.catch(e => console.log(e));
// ["hello", Error: 报错了]


如果p2没有自己的catch方法，就会调用Promise.all()的catch方法。
const p1 = new Promise((resolve, reject) => {
  resolve('hello');
})
.then(result => result);

const p2 = new Promise((resolve, reject) => {
  throw new Error('报错了');
})
.then(result => result);

Promise.all([p1, p2])
.then(result => console.log(result))
.catch(e => console.log(e));
// Error: 报错了
应用场景：依赖两个或多个的返回状态才能继续向下执行
```


<strong>5.promise.race()</strong>
&emsp;&emsp; `race` 表示所有 Promise 对象只要有返回状态就继续执行
```
const p1 = new Promise((resolve,reject)=>{
    setTimeout(()=>{
        resolve('等待3秒之后执行')
    },3000)
})

const p2 = new Promise((resolve,reject)=>{
    setTimeout(()=>{
        resolve('等待1秒之后执行')
    },1000)
})
Promise.race([p1,p2]).then(res=>{
    console.log(res);   //返回的是最先成功的状态
    console.log('只要其中任意一个有状态我就显示');
})
应用场景：做一个容错，同时请求两个或多个接口，只要有返回状态就可以继续向下执行
```


<strong>6.primise.prototype.then()</strong>
&emsp;&emsp; Promise 实例具有 `then` 方法，也就是说，`then` 方法是定义在原型对象，`then` 方法的第一个参数是 `resolved` 状态的回调函数，第二个参数（可选）是 `rejected` 状态的回调函数。`then` 方法返回的是一个新的Promise实例（注意，不是原来那个Promise实例）。因此可以采用链式写法，即 `then` 方法后面再调用另一个 `then` 方法。
```
new Promise((resolve, reject) => {
    resolve(1);
    console.log(2);
}).then(r => {
    console.log(r);
})
//输出顺序为 2，1
```


<strong>7.promise.prototype.catch()</strong>
&emsp;&emsp; `Promise.prototype.catch` 方法是 `.then(null, rejection)` 的别名，用于指定发生错误时的回调函数。
```
const promise = new Promise(function(resolve, reject) {
  throw new Error('test');
});
promise.catch(function(error) {
  console.log(error);
});
注释：不要在then方法里面定义 Reject 状态的回调函数（即then的第二个参数），总是使用catch方法。

    //不推荐
promise
    .then(function(data) {
        // success
    }, function(err) {
        // error
    });
    
    //推荐使用
promise
    .then(function(data) {
    /   / success
    })
    .catch(function(err) {
        // error
    });

    
//下面代码没有报错，跳过了catch方法，直接执行后面的then方法。此时，要是then方法里面报错，就与前面的catch无关
Promise.resolve()
.catch(function(error) {
  console.log('oh no', error);
})
.then(function() {
  console.log('carry on');
});
```
<strong>8.promise.prototype.finally()</strong>


### ECMAScript7


#### includes()

&emsp;&emsp;ES7新特性，在ES6的基础上新增二项内容分别为

1.includes()的作用，是检索某个值是否存在数组里，若存在则返回true,反之则返回false。
Array.peototype.includes()方法 || String.peototype.includes()方法
```
	const newArr = ['a', 'b', 'c'];
	newArr.includes('a')// true || newArr.includes('d')// false
	const newStr = 'abcdefg';
	newStr.includes('a')//true || newStr.includes('d')//false

```
注：includes()和indexOf()方法优缺点
简便性
&emsp;&emsp;从这一点上来说，includes略胜一筹。熟悉indexOf的同学都知道，indexOf返回的是某个元素在数组中的下标值，若想判断某个元素是否在数组里，我们还需要做额外的处理，即判断该返回值是否>-1。而includes则不用，它直接返回的便是Boolean型的结果。
精确性
&emsp;&emsp;两者使用的都是 === 操作符来做值的比较。但是includes()方法有一点不同，两个NaN被认为是相等的，即使在NaN === NaN结果是false的情况下。这一点和indexOf()的行为不同，indexOf()严格使用===判断。
```
	console.log(NaN===NaN)   //特殊情况为false
	let demo = [1, NaN, 2, 3]
	demo.indexOf(NaN)        //-1
	demo.includes(NaN)       //true
```
数组去重去除NaN方法
```
Array.prototype.uniq = function () {
    var result = []
    var hasNaN = false
    for(var i = 0; i < this.length; i++ ){
        var elem = this[i]

        if(result.indexOf(elem) == -1){
            if(elem !== elem){
                if(!hasNaN){
                    result.push(elem)
                    hasNaN = true
                }
            }else{
                result.push(elem)
            }
        }
    }
    return result
}
```
#### 求幂运算符(\*\*)
2.求幂运算符(\*\*)
基本用法3\*\*2&emsp;//9，效果同Mtah.pow(3,2)相同//9，效果同3<sup>2</sup>=9 
既然说\*\*是一个运算符，那么就应该能满足类似加(+)减(-)乘(*)除(/)等操作。

```
	let a = 3;
	a **= 2;	//9
```
ES的这个新特性是从其他语言（Python，Ruby等）模仿而来的。

### ECMAScript8

#### async函数

首先使用callback promise generator  async/await
由初始到最新的es6

```
function * test(){
   var target = yield 123456
}

saync function teste(){
    const module = await module(res)
    const model = await demo(err)
}
```












































