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

#### combineReducers
reducer函数负责生成State。由于整个应用只有一个State对象，包含所有数据，杜宇大型应用来说，整个State必然十分庞大，导致Reducer函数也十分庞大。
```
const chatReducer = (state = defaultState,action={}) => {
    const { type,payload } = action;
    switch(type){
        case ADD_CHAT:
            return Object.assign({},state,{ chatLog:state.chatLog.concat(payload) })
        case CHANGE_STATUS:
            return Object.assign({},state,{ statusMessage:payload })
        case CHANGE_USERNAME:
            return Object.assign({},state,{ userName:payload })
        default:
            return state;
    }
}
```
上面代码中，三种Action分别改变State的三个属性
```
· ADD_CHAT: chatLog属性
· CHANGE_STATUS: statusMessage属性
· CHANGE_USERNAME: userName属性
```
这三个属性之间没有联系，这提示我们可以把Reducer函数拆分。不同的函数负责处理不同属性，最终把他们合并成一个大Reducer即可。
```
const chatReducer = (state = defaultState,action={}) => {
    return {
        chatLog: chatLog(state.chatLog,action),
        statusMessage: statusMessage(state.statusMessage,action),
        userName: userNmae(state.userName,action)
    }
}
```
上面代码中，Reducer函数被拆成了三个小函数，每一个负责生成对应的属性。
这样一拆，Reducer就易读易写多了，而且，这种拆分于React应用的结构吻合：一个React跟组件由很多子组件构成。这就是说，子组件与子Reducer完全可以对应。
Redux提供了一个`combineReducers`方法，用于Reducer的拆分。你只要定义各个子Reducer函数，然后用这个方法，将他们合成一个大Reducer。
```
import { combineReducers } from 'redux'
const chatReducer = combineReducers({
    chatLog,
    statusMessage,
    userName
})
export default todoApp
```
上面的代码通过`combineReducers`方法将三个子Reducer合并成一个大的函数。
这种写法有一个前提，就是state的属性名必须与子Reducer同名。如果不同名，就要采用下面的写法
```
const reducer = combineReducers({
    a: doSomethingWitchA,
    b: processB
    c: c
})

==>

function reducer(state={},action){
    return {
        a: doSomethingWithA(stats.a,action),
        b: processB(state.b,action),
        c: c(state.c,action)
    }
}
```
你可以把所有的子Reducer放在一个文件里面，然后统一引入
```
import { combnineReducers } from 'redux'
import * as reducers from './reducers'

const reducer = combineReducers(reducers)
```

### Redux流程图

<img title="redux流程图" src="http://www.ruanyifeng.com/blogimg/asset/2016/bg2016091802.jpg" width="400"/>

### 简单实例：计数器

