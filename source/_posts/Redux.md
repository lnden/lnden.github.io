---
title: Redux
date: 2018-12-20 13:40:50
tags:
---

### 简介
&emsp;&emsp;众所周知React是一套UI层面的框架，可以高效的创建前端SPA应用，但并不是Web应用的完整解决方案。比如代码结构、组件之间的通信。对于大型的复杂应用来说，这两方面恰恰是最关键的。因此，只用 React 没法写大型应用。为了解决这个问题，2014年 Fecebook 提出了 Flux 架构的概念，引发了很多的实现，2015年 redux 出现，将 Flux 与函数式变成结合在一起，很短时间内就成为了最热门的前端架构。
&emsp;&emsp;你试想一下使用React构建一个大型应用，组件传递状态层次很深，这时候就需要使用Props一层一层的传递，写到最后就会变得一团糟，并且维护成本非常高。这个时候就需要使用redux来管理web应用的状态~
### 基础概念和API
#### store
store就是保存数据的地方，你可以把它看成一个容器。整个应用只能有一个Store
redux提供 `createStore` 这个函数，用来生成Store

```
import createStore from 'redux';
const store = createStore(fn );
```
上面代码中 `createStore` 函数接受另一个函数作为参数，返回新生成的Store对象


##### getState()
&emsp;&emsp;Store对象包含所有数据。如果想得到某个时点的数据，就要对Store生成快照。这种时点的数据集合，就叫做State。当前时刻的State，可以通过store.getState()拿到

##### dispatch()
&emsp;&emsp;dispatch()是View发出Action的唯一方法，dispatch()接受一个Action对象作为参数，将它发送出去。
```
import { createStore } from 'redux';
const store = createStore(fn);

import { ADD_TODO } from '../actionTypes.js'
store.dispatch({
    type:ADD_TODO,
    payload:'description'
})

/* 可以简化action */
store.dispatch(addTodo('description'))
```

##### subscribe()
Store允许使用store.subscribe方法设置监听函数，一旦State发生变化，就自动执行这个函数
```
import { createStore } from 'redux'
const store = createStore(reducer)

store.subscribe(listener)

let unsubscribe = store.subscribe(()=>{
    console.log('可以监听到state的变化，在这里可以获取到最新的state',store.getState())
})
unsubscribe()
```

#### Action
&emsp;&emsp;state的变化，会导致View的变化。但是，用户接收不到State,只能接触到View。所以，state的变化必须是View导致的。Action就是view发出的通知，表示state应该要发生变化了。
Action 是一个对象，其中的type属性是必须的，表示Action的名字。其他属性可以自由设置，可以参考社区规范。
```
const action = {
    type:ADD_TODO,
    payload:'Learn Redux'
}
```
上面代码中，Action的名字是`ADD_TODO`，它携带的信息是字符串Learn
注：可以这样理解，Action描述当前发生的事情，改变state的唯一办法，就是使用Action。它会运送数据到Store。

用户(View)要发送多少消息，就会有多少种Action。如果都手写，会很麻烦。可以定义个函数来生成Action，这个函数就叫Action Creator。

```
const ADD_TODO = 'ADD_TODO'
function addTodo(text){
    return {
        type:ADD_TODO,
        text
    }
}
const action = addTodo('learn Redux')

addTodo函数就是一个Action Creator。

```
#### Reducer
&emsp;&emsp;store收到Action以后，必须给出一个新的State，这样View才会发生变化。这种State的计算过程叫做Reducer。
Reducer是一个纯函数，它接受当前的State和Action作为参数，返回一个新的State

```
const reducer = function(previousState,action){
    // ...
    return newState
}
```
```
const ADD_TODO = 'ADD_TODO';
import { ADD_TODO } from '../actionTypes.js'

const defaultState = 0;
const reducer = (state=defaultState,action) =>{
    switch(action.type){
        case ADD_TODO:
            return state+action.payload;
        default:
            return state;
    }
}

const state = reducer(1,{
    type:ADD_TODO,
    PAYLOAD:2
})
```
&emsp;&emsp;上面代码中，reducer函数收到名为ADD_TODO的action以后，就返回一个新的State，作为加法的计算结果。其他运算的逻辑，也可以根据Action的不同来实现。
&emsp;&emsp;实际应用中，reducer函数不用像上面这样手动调用，store.dispatch方法会触发Reducer的自动执行。为此，store需要知道Reducer函数，做法就是在生成Store的时候，将Reducer传入createStore方法
```
import { createStore } from 'redux'
const store = createStore(reducer)
```

### Redux流程图

<img title="redux流程图" src="http://www.ruanyifeng.com/blogimg/asset/2016/bg2016091802.jpg" width="400"/>

### 简单实例：计数器

