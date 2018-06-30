---
title: Vue入门与实战
date: 2018-04-04 23:51:02
tags:
	- 前端框架
---
### 前言
&emsp;&emsp;Vue已经使用过一段时间了，今天有时间把以前学习的demo整理了一下，并且写一篇关于Vue的博客，以便以后使用的时候能够快速的找到问题所在。首先我来介绍一下vue，vue是一个js的MVVM框架、vue的核心只关注视图层、vue轻量级并且易于上手、也是开源世界华人的骄傲，因为它的作者是位中国人–尤雨溪（Evan You）。如果你想学习Vue，从一个新人小白到可以搭建一个复杂的页面应用(SPA)，那从今天开始你要坚持，努力的去学习、练习。
&emsp;&emsp;这是一篇从零开始的基础教程，前期的基础会让你觉得枯燥乏味，等你了解基础以后，把所有的知识串联到一起，去实现一个单页面应用的时候，你会感觉无比的自豪。
### Vue入门

#### vue指令
##### v-module表单输入绑定
你可以用 v-model 指令在表单 <input> 及 <textarea> 元素上创建双向数据绑定。v-model 本质上不过是语法糖。
```
<input v-model="message" placeholder="edit me">
<p>Message is: {{ message }}</p>
```
```
<span>Multiline message is:</span>
<p style="white-space: pre-line;">{{ message }}</p>
<br>
<textarea v-model="message" placeholder="add multiple lines"></textarea>
```
在文本区域插值 (<textarea></textarea>) 并不会生效，应用 v-model 来代替。
##### v-show
##### v-if
##### v-else
##### v-else-if
##### v-on
可以用 v-on 指令监听 DOM 事件，并在触发时运行一些 JavaScript 代码。

v-on 可以接收一个需要调用的方法名称和内联处理方法。
```
<div id="example-2">
    <!-- `counter` 可以运行一些js代码 -->
    <button v-on:click="counter += 1">Add 1</button>
    
    <!-- `greet` 可以定义的方法名 -->
    <button v-on:click="greet">Greet</button>
    
    <!-- `say` 可以传递参数() -->
    <button v-on:click="say('hi')">Say hi</button>
    
    <!-- `say` 可以传入事件对象$event -->
    <button v-on:click="say('what',$event)">Say what</button>
</div>
```
```
var example2 = new Vue({
  el: '#example-2',
  data: {
    counter: 0,
    name: 'Vue.js'
  },
  // 在 `methods` 对象中定义方法
  methods: {
    greet: function (event) {
      // `this` 在方法里指向当前 Vue 实例
      alert('Hello ' + this.name + '!')
      // `event` 是原生 DOM 事件
      if (event) {
        alert(event.target.tagName)
      }
    },
    say(str,e){ }
  }
})

// 也可以用 JavaScript 直接调用方法
example2.greet() // => 'Hello Vue.js!'
```
事件修饰符(如初次学习本教程可以略过以下)
`event.preventDefault()` 或 `event.stopPropagation()` 是非常常见的
```
.stop
.prevent
.capture
.self
.once
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>
```
按键修饰符

在监听键盘事件时，我们经常需要检查常见的键值。Vue 允许为 v-on 在监听键盘事件时添加按键修饰符

<!-- 只有在 `keyCode` 是 13 时调用 `vm.submit()` -->
<input v-on:keyup.13="submit">
<!-- 同上 -->
<input v-on:keyup.enter="submit">

<!-- 缩写语法 -->
<input @keyup.enter="submit">

##### v-for
我们用 v-for 指令根据一组数组的选项列表进行渲染。v-for 指令需要使用 item in items 形式的特殊语法，items 是源数据数组并且 item 是数组元素迭代的别名。
1.用v-for 把一个数组对应为一组元素
```
<ul id="example-1">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ul>

//可以使用 of 代替 in 作为分隔符，这样更接近JS迭代器的语法
<div v-for="item of items"></div>

//还支持一个可选的第二个参数为当前项的索引
<ul id="example-2">
  <li v-for="(item, index) in items">
    {{ index }} - {{ item.message }}
  </li>
</ul>
```
```
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```
2.用v-for 循环一个对象
```
<ul id="" class="demo">
  <li v-for="value in object">
    {{ value }}
  </li>
</ul>
```
```
new Vue({
  el: '#v-for-object',
  data: {
    object: {
      firstName: 'John',
      lastName: 'Doe',
      age: 30
    }
  }
})
```
也可以提供第二个的参数为键名
```
<div v-for="(value, key) in object">
  {{ key }}: {{ value }}
</div>
```
3.key的使用
&emsp;&emsp;vue1.0中使用的方式 track-by="$index" 。
&emsp;&emsp;vue2.0中使用的方式 v-bind:key="index" / :key="item.id"

4.显示过滤/排序结果
&emsp;&emsp;有时，我们想要显示一个数组的过滤或排序副本，而不实际改变或重置原始数据。在这种情况下，可以创建返回过滤或排序数组的计算属性。
```
<li v-for="n in evenNumbers">{{ n }}</li>
```
```
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
computed: {
  evenNumbers: function () {
    return this.numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```
在计算属性不适用的情况下 (例如，在嵌套 v-for 循环中) 你可以使用一个 method 方法
```
<li v-for="n in even(numbers)">{{ n }}</li>
```
```
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
methods: {
  even: function (numbers) {
    return numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```
**<span style="color:#f00">注意</span>**：当在组件中使用 v-for 时，key 现在是必须的。
##### v-bind
它是一个 vue 指令，用于绑定 html 属性
1.可以绑定执行运算
2.可以绑定执行函数
##### v-text
##### v-html
##### v-once
##### v-cloak

#### 计算属性和监听器

##### 计算属性computed
1.computed基础使用
模板内的表达式非常便利，但是设计它们的初衷是用于简单运算的。在模板中放入太多的逻辑会让模板过重且难以维护。例如：
```
<template>
    <div id="example">
        {{ message }}
        {{ message.split('').reverse().join('') }}
    </div>
</template>
<script>
    export default {
        data(){
            return {
                message:'HELLO'
            }
        }
    }
</script>

```
在这个地方，模板不再是简单的声明式逻辑。你必须看一段时间才能意识到，这里是想要显示变量 message 的翻转字符串。当你想要在模板中多次引用此处的翻转字符串时，就会更加难以处理。

所以，对于任何复杂逻辑，你都应当使用计算属性。如下
```
<template>
    <div id="example">
        {{ message }}
        {{ reversedMessage }}
    </div>
</template>
<script>
    export default {
        data(){
            return {
                message:'HELLO'
            }
        },
        computed
            // 计算属性的 getter
            reversedMessage(){// 
                `this` 指向 vm 实例
                return this.message.split('').reverse().join('')
            }
        }
    }
</script>
```
2.computed计算属性缓存 vs methods方法
使用方法也可以达到同样的效果
```
<p>Reversed message: "{{ reversedMessage() }}"</p>

methods: {
  reversedMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
```
使用计算属性和方法都可以实现最终相同的结果。
区别：
&emsp;&emsp;计算属性是基于它们的依赖进行缓存的，只有相关依赖 `message` 发生改变时才会重求值。
&emsp;&emsp;方法是每当触发重新渲染时，调用方法将会再次执行函数。
为什么要缓存？
&emsp;&emsp;假设我们有一个性能开销比较大的的计算属性 A，它需要遍历一个巨大的数组并做大量的计算。然后我们可能有其他的计算属性依赖于 A 。如果没有缓存，我们将不可避免的多次执行 A 的 getter！如果你不希望有缓存，请用方法来替代。
3.computed计算属性 vs watch侦听属性
&emsp;&emsp;禁止滥用watch，通常更好的做法是使用计算属性而不是命令式的 watch 回调。
```
<div id="demo">{{ fullName }}</div>
```
使用watch监听属性
```
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
```
使用computed计算属性
```
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})
```
4.计算属性的 setter
&emsp;&emsp;计算属性默认只有 getter ，不过在需要时你也可以提供一个 setter ：
```
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```
现在再运行 vm.fullName = 'John Doe' 时，setter 会被调用，vm.firstName 和 vm.lastName 也会相应地被更新。
##### 监听器watch
&emsp;&emsp;可以监听数据的变化，可以监听路由的变化，可以监听vuex的状态
```
    watch: {
        question: function (newQuestion, oldQuestion) {
            //监听 `question` 数据发生改变
            this.answer = 'Waiting for you to stop typing...'
            this.getAnswer()
        },
        $route(){
            //监听路由改变
        },
        "$store.state.typeName"(){
            //监听vuex状态的改变
        }
    }
   
```
watch-实例应用
```
<div id="watch-example">
  <p>
    Ask a yes/no question:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>
```
```
<script src="https://cdn.bootcss.com/vue/2.5.17-beta.0/vue.js"></script>
<script src="https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js"></script>
<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    // 如果 `question` 发生改变，这个函数就会运行
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.getAnswer()
    }
  },
  methods: {
    // `_.debounce` 是一个通过 Lodash 限制操作频率的函数。
    // 在这个例子中，我们希望限制访问 yesno.wtf/api 的频率
    // AJAX 请求直到用户输入完毕才会发出。想要了解更多关于
    // `_.debounce` 函数 (及其近亲 `_.throttle`) 的知识，
    // 请参考：https://lodash.com/docs#debounce
    getAnswer: _.debounce(
      function () {
        if (this.question.indexOf('?') === -1) {
          this.answer = 'Questions usually contain a question mark. ;-)'
          return
        }
        this.answer = 'Thinking...'
        var vm = this
        axios.get('https://yesno.wtf/api')
          .then(function (response) {
            vm.answer = _.capitalize(response.data.answer)
          })
          .catch(function (error) {
            vm.answer = 'Error! Could not reach the API. ' + error
          })
      },
      // 这是我们为判定用户停止输入等待的毫秒数
      500
    )
  }
})
</script>
```

#### Class与Style绑定

##### Class的使用
一、对象语法
1.绑定一个对象
```
<div v-bind:class="{ active : isActive }"></div>    //active 这个 class 存在与否将取决于数据属性 isActive 的true/false
```
2.v-bind:class 指令也可以与普通的 class 属性共存
```
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>
data: {
  isActive: true,
  hasError: false
}
```
```
渲染的结果为
<div class="static active"></div>
```
3.绑定的对象不必使用内联定义在模板里面
```
<div v-bind:class="classObject"></div>
```
```
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```
4.还可以绑定一个返回对象的计算属性
```
<div v-bind:class="classObject"></div>
```
```
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```
二、数组语法
1.可以绑定一个数组
```
<div v-bind:class="[activeClass, errorClass]"></div>
```
```
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```
2.使用三元运算符
```
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```
3.数组中也可以使用对象语法
```
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```
##### Style的使用
一、对象语法
1.绑定使用内联的方式
```
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```
```
data: {
  activeColor: 'red',
  fontSize: 30
}
```
2.绑定使用对象的方式
```
<div v-bind:style="styleObject"></div>
```
```
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```
3.也可以使用计算属性
二、数组语法
1.绑定一个数组
```
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```
2.多重值
```
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>    //这样写只会渲染数组中最后一个被浏览器支持的值
```
#### 渲染条件
##### v-if
##### v-show
##### v-for

#### Vue的组件	
	
#### Vue的声明周期

### Vue-router路由

[简介](#vue-router简介) | [基本使用](#router基本使用) | [动态路由](#router动态路由) | [路由嵌套](#router路由嵌套) | [命名路由](#router命名路由) | [对象信息](#路由信息对象的属性)

#### router简介
庸俗点说,可以不需要刷新页面就可以跳到对应的板块,减少对服务器的请求次数
1，什么是前端路由？
&emsp;&emsp;路由是根据不同的 url 地址展示不同的内容或页面
&emsp;&emsp;前端路由就是把不同路由对应不同的内容或页面的任务交给前端来做，之前是通过服务端根据 url 的不同返回不同的页面实现的。
	
2，什么时候使用前端路由？
&emsp;&emsp;在单页面应用，大部分页面结构不变，只改变部分内容的使用
	
3，前端路由有什么优点和缺点
&emsp;&emsp;优点
&emsp;&emsp;&emsp;&emsp;用户体验好，不需要每次都从服务器全部获取，快速展现给用户
&emsp;&emsp;缺点
&emsp;&emsp;&emsp;&emsp;使用浏览器的前进，后退键的时候会重新发送请求，没有合理地利用缓存
&emsp;&emsp;&emsp;&emsp;单页面无法记住之前滚动的位置，无法在前进，后退的时候记住滚动的位置	

#### router基本使用
1.[main.js]首先你要引入 vue-router
```
import VueRouter from "vue-router"
```
	
2.[main.js]引入之后要使用 VueRouter
```
Vue.use(VueRouter)
```
	
3.[components]定义(组件)组件,新建需要的组件
```
-Introduce.vue
-Column.vue
-Detail.vue
```
	
4.[router/router.config.js]定义路由
```
import  columnOne  from  "../components/Introduce"
import  columnTwo  from  "../components/Column"
import  columnThird  from  "../components/Detail"

export default {
    routes : [
        {
            path:'*',
            redirect:'/recommed',   //redirect重定向,没事初始化页面都自动跳转当前路径
        },
        {
            path:'/recommed',       //路径[路径名称与router-link to标签对应]
            component:columnOne,    //对应组件
        },
        {
            path:'/column',
            component:columnTwo,
        },
        {
            path:'/detail',
            component:columnThird,
        }
    ]
}
每个路由应该对应映射一个组件component
```

5.[main.js]创建 router 实例，然后传 routes 配置
```
import routes from "./router/router/router.config.js"
const router = new VueRouter( routes )
```

6.[main.js]创建和挂载根实例
```
new Vue({
    router                //（缩写）相当于 routes: routes[es6写法]
    render:h => h(App)
}).$mount("#app")

这里实现一个Tab切换，每次切换路由展示对应的组件内容，三个页面内容各自不同。
可以在App.vue文件下使用<router-link>和<router-view>配合使用路由
<section>
    <ul>
        <router-link tag="li" to="/recommed" >推荐</router-link>
        <router-link tag="li" to="/column" >目录</router-link>
        <router-link tag="li" to="/detail">详情</router-link>
    </ul>
    <router-view></router-view>
</section>
```

#### router动态路由
我们经常需要把某种模式匹配到的所有路由，全都映射到同个组件。
例如，我们有一个 User 组件，对于所有 ID 各不相同的用户，都要使用这个组件来渲染。那么，我们可以在 vue-router 的路由路径中使用『动态路径参数』
*正常添加路由如上
1.[rouoter.config.js]更改路由配置文件
```
import User from "../components/User.vue"
import Home from "../components/Home.vue"
import About from "../components/About.vue"

export default {
    routes:[
        {
            path: '/',
            redirect: '/home'
        },
        {
            path:"/home",
            component: Home
        },
        {
            path: "/about",
            component: About
        },
        /*新增user路径，配置了动态的id*/
        {
            path: "/user/:id",
            component: User
        }
    ]
}
```
2.[App.vue]添加动态路由跳转

```
<section>
    <ul>
        <router-link tag="li" to="/home">主页</router-link>
        <router-link tag="li" to="/about">关于我们</router-link>
        <!--  增加两个到user组件的导航，可以看到这里使用了不同的to属性 -->
        <router-link tag="li" to="/user/login">登陆</router-link>
        <router-link tag="li" to="/user/register">注册</router-link>
    </ul>
    <router-view></router-view>
</section>
```
3.[User.vue]可以使用computed:{...},watch:{...}监测路由的改变
```
<template>
    <div>
        <h1>User</h1>
        <div>我是user组件, 动态部分是{{dynamicSegment}}</div>
    </div>
</template>

<script>
    export default {
        computed: {
            dynamicSegment () {
                return this.$route.params.id
            }
        },
        data(){
            return {
                dynamicSegment:''
            }
        },
        watch:{
            $route(to,from){
                //to表示的是你要去的那个组件，from表示的是你从哪个组件过来的
                //它们是两个对象，你可以把它打印出来，它们也有一个param 属性
                console.log(to);
                console.log(from);
                this.dynamicSegment = to.params.id
            }
        }
    }
</script>
```

#### router路由嵌套
嵌套路由，主要是由我们的页面结构所决定的。当我们进入到home页面的时候，它下面还有分类，如手机系列，平板系列，电脑系列。当我们点击各个分类的时候，它还是需要路由到各个部分，如点击手机，它肯定到对应到手机的部分。在路由的设计上，首先进入到 home ,然后才能进入到phone, tablet, computer.  Phone, tablet, compute 就相当于进入到了home的子元素。
所以vue  提供了childrens 属性，它也是一组路由,相当于我们所写的routes。首先，在home页面上定义三个router-link标签用于导航，然后再定义一个router-view标签，用于渲染对应的组件。
注：router-link 和router-view 标签要一一对应。
1.[Home.vue]添加内容如下
```
<template>
    <section>
        <h1>这是主页11111111111</h1>
        <!-- router-link 的to属性要注意，路由是先进入到home,然后才进入相应的子路由如 phone,所以书写时要把 home 带上 -->
        <ul>
            <router-link tag="li" to="/home/phone">手机</router-link>
            <router-link tag="li" to="/home/tablet">平板</router-link>
            <router-link tag="li" to="/home/computer">电脑</router-link>
        </ul>
        <router-view></router-view>
    </section>
</template>

<script>
    export default {}
</script>
```
2.[router.config.js]更改路由配置文件
```
import home from "../components/Home.vue"
import discover from "../components/Discover.vue"
import about from "../components/Abount.vue"
import personal from "../components/Personal.vue"

//该嵌套组件放置在Home文件下也可以执行
import phone from "../components/Home/Phone.vue"
import tablet from "../components/Home/Tablet.vue"
import computer from "../components/Home/Computer.vue"

export default {
    routes:[
        {
            path:'/',
            redirect:'/home'
        },
        {
            path:'/home',
            component:home,
        //这个是路由嵌套的更新，注意：path路由只是名称，没有/
        children:[
            {
                path:'phone',
                component:phone
            },
            {
                path:'tablet',
                component:tablet
            },
            {
                path:'computer',
                component:computer
            },
            // 当进入到home时，下面的组件显示
            {
                path: "",
                component: phone
            }
        ]
        },
        {
            path:'/discover',
            component:discover
        },
        {
            path:'/about',
            component:about
        },
        {
            path:'/personal',
            component:personal
        }
    ]
}
```

#### router命名路由
有时候，通过一个名称来标识一个路由显得更方便一些，特别是在链接一个路由，或者是执行一些跳转的时候。
你可以在创建 Router 实例的时候，在 routes 配置中给某个路由设置名称。

1.要链接到一个命名路由，可以给 router-link 的 to 属性传一个对象
```
<li><router-link :to="{ name: 'home' }">home</router-link></li>
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
这跟代码调用 router.push() 是一回事
router.push({ name: 'user', params: { userId: 123 }})

import Vue from 'vue'
import VueRouter from 'vue-router'
Vue.use(VueRouter)

const Home = { template: '<div>This is Home</div>' }
const Foo = { template: '<div>This is Foo</div>' }
const Bar = { template: '<div>This is Bar {{ $route.params.id }}</div>' }

const router = new VueRouter({
    mode: 'history',
    base: __dirname,
    routes: [
        { path: '/', name: 'home', component: Home },
        { path: '/foo', name: 'foo', component: Foo },
        { path: '/bar/:id', name: 'bar', component: Bar }
    ]
})

new Vue({
    router,
    template: `
        <div id="app">
        <h1>Named Routes</h1>
        <p>Current route name: {{ $route.name }}</p>
        <ul>
        <li><router-link :to="{ name: 'home' }">home</router-link></li>
        <li><router-link :to="{ name: 'foo' }">foo</router-link></li>
        <li><router-link :to="{ name: 'bar', params: { id: 123 }}">bar</router-link></li>
        </ul>
        <router-view class="view"></router-view>
        </div>
    `
}).$mount('#app')
```

#### 路由信息对象的属性
```
1.$route.params
    类型: Object
    一个 key/value 对象，包含了 动态片段 和 全匹配片段，如果没有路由参数，就是一个空对象。
    this.$route.params.type     //获取url里面的参数
2.$route.path
    类型: string
    字符串，对应当前路由的路径，总是解析为绝对路径
    this.$route.path    //获取路径地址 "/foo/bar"。
3.$route.query
    类型: Object
    一个 key/value 对象，表示 URL 查询参数。
    例如，对于路径 /foo?user=1，则有 $route.query.user == 1，如果没有查询参数，则是个空对象。
4.$route.hash
    类型: string
    当前路由的 hash 值 (带 #) ，如果没有 hash 值，则为空字符串。
5.$route.fullPath
    类型: string
    完成解析后的 URL，包含查询参数和 hash 的完整路径。
6.$route.matched
    类型: Array<RouteRecord>
    一个数组，包含当前路由的所有嵌套路径片段的 路由记录 。
    路由记录就是 routes 配置数组中的对象副本（还有在 children 数组）。
    const router = new VueRouter({
        routes: [
            // 下面的对象就是 route record
            { 
                path: '/foo', component: Foo,
                children: [
                    // 这也是个 route record
                    { path: 'bar', component: Bar }
                ]
            }
        ]
    })
7.$route.name
    当前路由的名称，如果有的话。
```

#### routes里面参数
```
mode: 'history',    //去掉#使用的是h5的历史
base: __dirname,
```
---

### Vuex状态管理

vuex是一个专门为vue.js设计的集中式状态管理架构

#### vuex核心组成
1.state
vuex的单一状态树，使用一个对象就包含了应用层的所有状态。
我的理解是，state是vuex自己维护的一份状态数据
2.mutations
&emsp;&emsp;更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。vuex中的mutation 类似于事件，每个mutation都有一个字符串 事件类型(type)和一个回调函数(handler)。mutation中写有修改数据的逻辑。另外mutation里只能执行同步操作。
```
import { mapMutations } from 'vuex'

export default {
  // ...
  methods: {
    ...mapMutations([
      'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`

      // `mapMutations` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
    ]),
    ...mapMutations({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
    })
  }

```
3.actions
&emsp;&emsp;action 类似于 mutation，不同在于：action 提交的是 mutation，而不是直接变更状态。action 可以包含任意异步操作。
其实action和mutation类似，但是action提交是mutation，并不直接修改数据，而是触发mutation修改数据。
```
import { mapActions } from 'vuex'

export default {
  // ...
  methods: {
    ...mapActions([
      'increment', // 将 `this.increment()` 映射为 `this.$store.dispatch('increment')`

      // `mapActions` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.dispatch('incrementBy', amount)`
    ]),
    ...mapActions({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.dispatch('increment')`
    })
  }
}
```
4.getters
getters属性主要是对于state中数据的一种过滤，属于一种加强属性。比如你在做一个todolist，对于已完成的，你可以写一个doneTodoList的属性，在外面直接调用。其实他就是对于action和mutations的一个简化。不然你写一个doneTodoList功能，你还得写对应的action和mutation，费劲啊。
5.modules
module,模块化。
因为随着后面的业务逻辑的增多，把vuex分模块的开发会使得代码更加简洁清晰明了，比如登录一个模块，产品一个模块，这样后面改动起来也简单嘛。
#### vuex的基本使用
1.安装vuex
```
npm install vuex --save
```

2.引入并且使用vuex
```
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
```
3.在main.js 中引入新建的vuex文件
```
import store from './store/store.js'
```
4.store目录结构
```
-actions.js
-getters.js
-mutations.js
-state.js
-store.js
```
store.js里面可以他们都引入进来
```
import Vue from "vue";
import Vuex from "Vuex";
Vue.use(Vuex);

import state from "./state.js";
import mutation from "./mutations.js";
import actions from "./actions.js";
import getters from "./getters.js";

export default new Vuex.Store ({
    state,
    mutation,
    actions,
    getters,
})
```
其他文件内均export名称
```
const state = {
    ajaxing:false
};
export default state;

const mutaions = {

};
export default mutaions;

const actions = {
    accept:({commit, state},date)=>{
        state.bookstate = date;
    }
};
export default actions;

const getters = {
    contentDelete(state){
        return state.contentDelete;
    }
};
export default getters;
```
5.在实例化 Vue对象时加入 store 对象
```
new Vue({
    el: '#app',
    store,    			//在这里把store添加到vue实例上
    template: '<App/>',
    components: { App }
})
```
### Vue实战
