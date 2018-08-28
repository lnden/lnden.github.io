---
title: Vue性能优化
tags:
---

### 前言
&emsp;&emsp;其实vue运行时的性能我们不必担心，已经非常棒了！但是首屏加载时间以及加载后的SEO优化，我们还是要考虑的，让我们在vue性能优异的基础上更进一步。这里做一个总结，整合优化方案，以及项目实战中的性能优化。
&emsp;&emsp;来，先来看看我手指的方向(右侧)，阅读以下本文的目录，做一个大概的了解，这样阅读起来你的思路才会清晰明了！


### 首屏加载优化

#### 按需加载
#### 过渡动画
#### 应用CDN资源
#### 异步组件
#### 骨架屏

### SEO优化
#### 服务器端渲染SSR

### 项目优化

由于vue构建的项目越来越大，难以维护，所以前期一定要规划好，严格按照标准进行开发。


#### 编写规则

1.组件命名方式

    1).首字母大写

    2).命名语义化

    3).公共组件规范开发Common/Public

2.组件封装严格按照高内聚低耦合原则，拆分到当前组件下components，抽离公共组件。

3.组件内请求分离，api文件内请求对应views/pages组件内容(净化页面请求，把请求处理统一放在api内处理)

4.文件内添加好备注信息

    /**
     * @author Lnden
     * @date 2018/8/23
     * @desc File Description
     * @param {Object} [title]  - 参数说明
     * @example 调用示例
     */

5.vuex严格按照官方推荐方式，并且拆分五部分

    ├─store
    │  ├─index.js
    │  ├─getters.js
    │  ├─state,js
    │  ├─actions.js
    │  ├─mutations.js
    │  ├─mutation-types.js
6.图标统一使用的是svg和[阿里矢量图标库](http://www.iconfont.cn/)使用方式：symbol引用。组件注入vue跟实例上，直接使用无需引入


7.页面样式禁止使用!important;

8.禁止控制台输出 [黄色] 以及 [红色] 警告和报错信息

9.代码缩进，使用4个空格作为一个缩进层级(请调试好自己使用的编辑器)

10.如果使用第三方插件或者是库，npm install ...  --save-dve/-D 之后，请在README.md中进行备注

#### 基础优化

1.v-if和v-show的使用场景
&emsp;&emsp;第一个维度是权限问题，只要涉及到权限相关的展示无疑要用v-if
&emsp;&emsp;第二个维度在没有权限限制下根据用户点击的频次选择，频繁切换的使用 v-show，不频繁切换的使用 v-if

2.不要在模板里面写过多的判断与表达式

    虽然可以识别，但是不宜读，不好维护，建议封装在methods或者computed里面，便于其他相同地方复用
    v-if="isFirst && isAadmin && (a||b)"

2.使用v-for循环时绑定key唯一值

    用v-for 正在更新已渲染过的元素列表时，它默认用“就地复用”策略。
    如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序， 而是简单复用此处每个元素，
    并且确保它在特定索引下显示已被渲染过的每个元素
    <ul>
        <li v-for="(item,index) dataList" :key="index"></li>
    </ul>

3.对路由组件进行懒加载

    这里的懒加载是指在访问到对应的组件时才加载它，首屏的时候不加载
    const Login = () => import('@/pages/Login')
    { path: '/login', component: () => import('@/views/login/Login'), hidden: true }

4.打包优化
&emsp;&emsp;打包 vender 时不打包 vue、vuex、vue-router、axios 等，换用国内的 bootcdn 直接引入到根目录的 index.html 中。

    <script src="//cdn.bootcss.com/vue/2.2.5/vue.min.js"></script>
    <script src="//cdn.bootcss.com/vue-router/2.3.0/vue-router.min.js"></script>
    <script src="//cdn.bootcss.com/vuex/2.2.1/vuex.min.js"></script>
    <script src="//cdn.bootcss.com/axios/0.15.3/axios.min.js"></script>

在webpack里面有一个externals,可以忽略不需要打包的库

    externals: {
      'vue': 'Vue',
      'vue-router': 'VueRouter',
      'vuex': 'Vuex',
      'axios': 'axios'
    }

5.异步组件的方式

    <app>
        <A></A>
        <B v-if="showB"><B/>
        <C v-if="showC"><C/>
    </app>

    new Vue({
        data:{
            showB:false,
            showC:false
        },
        created(){
            //显示B
            setTimeout(()=>{
                this.showB = true;
            },0);
            //显示c
            setTimeout(()=>{
                this.showC = true;
            },0);
        }
    })
    因为v-if是惰性的，只有当第一次值为true时才会开始初始化。

6.按需加载

&emsp;&emsp;当前流行的UI框架如iview,muse-ui,Element UI都支持按需加载,只需稍微改动一下代码.

修改前

    import ElementUI from 'element-ui';
    import 'element-ui/lib/theme-chalk/index.css';
    Vue.use(ElementUI)

修改后
借助 `babel-plugin-component`，我们可以只引入需要的组件，以达到减小项目体积的目的。

    npm install babel-plugin-component -D

然后，将 .babelrc 修改为：

    {
      "presets": [["es2015", { "modules": false }]],
      "plugins": [
        [
          "component",
          {
            "libraryName": "element-ui",
            "styleLibraryName": "theme-chalk"
          }
        ]
      ]
    }

    import { Button, Select } from 'element-ui';

    Vue.component(Button.name, Button);
    Vue.component(Select.name, Select);
    /* 或写为
     * Vue.use(Button)
     * Vue.use(Select)
     */


7.服务端开启 gzip压缩
&emsp;&emsp;需要后台配置，待补充

### 总结

重点https://juejin.im/entry/58df274f61ff4b006b122948