---
title: Vue性能优化
tags:
---

## 前言
&emsp;&emsp;其实vue运行时的性能我们不必担心，已经非常棒了！但是首屏加载时间以及加载后的SEO优化，我们还是要考虑的，让我们在vue性能优异的基础上更进一步。这里做一个总结，整合优化方案，以及项目实战中的性能优化。
&emsp;&emsp;来，先来看看我手指的方向(右侧)，阅读以下本文的目录，做一个大概的了解，这样阅读起来你的思路才会清晰明了！


## 首屏加载优化

### 按需加载
这里我们只谈配合vue使用的UI框架，目前流行的有

&emsp;&emsp;1).Element-UI 饿了么vue2.0后台UI框架

&emsp;&emsp;2).iview 主要服务于 PC 界面的中后台业务

&emsp;&emsp;3).Mint-UI 饿了么移动端组件库

我们以 Element-UI 为例实现按需引入，减少项目的体积，减少加载时间，这里我就不介绍完整引入的方式以及vue搭建项目了，你能考虑到按需引入，想必你也是在对项目进行优化，下来让我们实现它！

1.安装 `babel-plugin-component` 插件

    cnpm install babel-plugin-component -D

2.修改根目录下 `.babelrc` 文件

    "plugins": [
        [
            "component",
            {
                "libraryName": "element-ui",
                "styleLibraryName": "theme-chalk"
            }
        ]
    ]
    //请认真的阅读代码格式

3.接下来引入你要使用的组件在 `main.js` 里面

    import { Button,Dialog } from 'element-ui';

    Vue.use(Button);    ||     Vue.component(Button.name, Button);
    Vue.use(Dialog);    ||     Vue.component(Dialog.name, Dialog);

到这里就算引入完成了，这是在 `main.js` 里面统一引入的，挂在到vue的实例上，你也可以在组件内部单独使用某些组件(但是不推荐样使用)

    <!--Login.vue-->
    <template>
        <section class="loginInner">
            <Button type="primary">组件内使用</Button>
        </section>
    </template>

    import { Button,Dialog } from 'element-ui';

    components:{
        Button,         ==> elButton:Button
        Dialog          ==> elDialog:Dialog
    }

### 过渡动画


过渡动画顾名思义，就是前端在加载页面的时候，设计一种划入划出、渐隐渐显的动画效果。这里我们使用vue推荐的方式 `transition` 的封装组件。

    <transition name="slide-fade">
        <p v-if="show">hello</p>
    </transition>

具体实施的动画样式，全部写在单独的动画样式文件内，以便日后维护添加新的动画效果

关于动画页面主流的还有loading图和进度条两种，这里我们稍微在介绍

### 引用CDN资源

CDN的全称是Content Delivery Network，即内容分发网络。
使内容传输的更快、更稳定。其目的是使用户可就近取得所需内容，解决 Internet网络拥挤的状况，提高用户访问网站的响应速度。减少服务器的压力等。如果你不了解CDN请自行百度吧！
在vue项目中，引入到项目中的所有js、css文件，编译时都会被打包进vendor.js，浏览器在加载该文件之后才能开始显示首屏。若是引入的库众多，那么vendor.js文件体积将会相当的大，影响首开的体验。

1.资源引入

    <body>
        <div id="app"></div>

        <script src="https://cdn.bootcss.com/vue/2.5.2/vue.min.js"></script>
        <script src="https://cdn.bootcss.com/vue-router/3.0.1/vue-router.min.js"></script>
        <script src="https://cdn.bootcss.com/vuex/3.0.1/vuex.min.js"></script>
    </body>

2.添加配置

    module.exports = {
        entry: {
            app: './src/main.js'
        },
        externals:{
            'vue': 'Vue',
            'vue-router': 'VueRouter',
            'vuex':'Vuex'
        }
    }
新增externals，将引用的外部模块导入。

注意：格式为 'aaa' : 'bbb', 其中，aaa表示要引入的资源的名字，bbb表示该模块提供给外部引用的名字，由对应的库自定。例如，vue为Vue，vue-router为VueRouter。

3.去掉原有的引用，重新npm run build，会看到vendor.js体积有所下降，通过nextwork工具可以看到vue.js/vuex.js/vendor.js等文件会分别由一个线程进行加载。
### 异步组件
### 骨架屏

#### 骨架屏概念
骨架屏就是在页面内容未加载完成的时候，先使用一些图形进行占位，待内容加载完成之后再把它替换掉。这个技术在一些以内容为主的APP和网页应用较多，接下来我们以一个简单的Vue工程为例，一起探索如何在基于Vue的SPA项目中实现骨架屏。

为了简单起见，我们使用 `vue-cli` 搭配 `webpack-simple` 这个模板来新建项目：

    vue init webpack-simple vue-skeleton

初始化之后我们安装依赖 `npm install` 安装完成之后启动该项目 ` npm run dev`，一个简单的单页应模板我们就搭建好了，打开index.html。

    <!DOCTYPE html>
    <html lang="en">
        <head>
            <meta charset="utf-8">
            <title>vue-skeleton</title>
        </head>
        <body>
            <div id="app"></div>
            <script src="/dist/build.js"></script>
        </body>
    </html>

可以看到DOM里面仅有一个 `div#app` ，当js加载完成之后，此 `div#app` 会被整个替换掉，因此，我们可以来做一下实验，在此div里面添加一些内容

    <div id="app">
        <p>Hello skeleton</p>
        <p>Hello skeleton</p>
        <p>Hello skeleton</p>
    </div>

打开Chrome的开发者工具，在Nextwork里面找到throttle功能，调节网速为“Slow 3G”，刷新页面，就能看到页面先是展示了三句“Hello skeleton”，待js加载完了才会替换为原本要展示的内容。现在，我们对于如何在Vue页面实现骨架屏，已经有了一个很清晰的思路——在 `div#app` 内直接插入骨架屏相关内容即可。

到目前为止，可以在index.html内事先添加好结构和样式，但是这样不容易维护，感觉有点low，我们需要一个扩展性强且自动化的易维护方案。既然是在Vue项目里，我们当然希望所谓的骨架屏也是一个.vue文件，它能够在构建时由工具自动注入到 `div#app` 里面。

#### 易维护方案

1.首先，我们在 `/src` 目录下新建一个 `Skeleton.vue` 文件。在文件内写好需要展示的占位结构和样式

2.接下来，再新建一个 `skeleton.entry.js` 入口文件

    import Vue from 'vue'
    import Skeleton from './Skeleton.vue'

    export default new Vue({
      components: {
        Skeleton
      },
      template: '<skeleton />'
    })
在完成了骨架屏的准备之后，就轮到一个关键插件 `vue-server-renderer ` 登场了。该插件本用于服务端渲染，但是在这个例子里，我们主要利用它能够把.vue文件处理成html和css字符串的功能，来完成骨架屏的注入。

3.我们还需要在根目录新建一个 `webpack.skeleton.conf.js` 文件，以专门用来进行骨架屏的构建。

    const path = require('path')
    const webpack = require('webpack')
    const nodeExternals = require('webpack-node-externals')
    const VueSSRServerPlugin = require('vue-server-renderer/server-plugin')

    module.exports = {
        target: 'node',
        entry: {
            skeleton: './src/skeleton.entry.js'
        },
        output: {
            path: path.resolve(__dirname, './dist'),
            publicPath: '/dist/',
            filename: '[name].js',
            libraryTarget: 'commonjs2'
        },
        module: {
            rules: [
                {
                    test: /\.css$/,
                    use: [
                        'vue-style-loader',
                        'css-loader'
                    ]
                },
                {
                    test: /\.vue$/,
                    loader: 'vue-loader'
                }
            ]
        },
        externals: nodeExternals({
            whitelist: /\.css$/
        }),
        resolve: {
            alias: {
                'vue$': 'vue/dist/vue.esm.js'
            },
            extensions: ['*', '.js', '.vue', '.json']
        },
        plugins: [
            new VueSSRServerPlugin({
                filename: 'skeleton.json'
            })
        ]
    }

    cnpm install webpack-node-externals -D
    cnpm install vue-server-renderer -D

可以看到，该配置文件和普通的配置文件基本完全一致，主要的区别在于其 `target: 'node'` ，配置了 `externals` ，以及在 `plugins` 里面加入了 `VueSSRServerPlugin`。在 `VueSSRServerPlugin` 中，指定了其输出的json文件名。我们可以通过运行下列指令，在 `/dist` 目录下生成一个 `skeleton.json` 文件，配置 `package.json` 文件内 `"script"`

    "generate": "webpack --config ./webpack.skeleton.conf.js"
    //配置完成之后
    npm run generate
    //dist文件下生成skeleton.json
    //这个文件在记载了骨架屏的内容和样式，会提供给vue-server-renderer使用。

4.接下来，在根目录下新建一个 `skeleton.js`，该文件即将被用于往index.html内插入骨架屏。

    const fs = require('fs')
    const { resolve } = require('path')

    const createBundleRenderer = require('vue-server-renderer').createBundleRenderer

    // 读取`skeleton.json`，以`index.html`为模板写入内容
    const renderer = createBundleRenderer(resolve(__dirname, './dist/skeleton.json'), {
        template: fs.readFileSync(resolve(__dirname, './index.html'), 'utf-8')
    })

    // 把上一步模板完成的内容写入（替换）`index.html`
    renderer.renderToString({}, (err, html) => {
        fs.writeFileSync('index.html', html, 'utf-8')
    })

注意，作为模板的html文件，需要在被写入内容的位置添加 `<!--vue-ssr-outlet-->` 占位符，本例子在 `div#app`里写入

    <div id="app">
        <!--vue-ssr-outlet-->
    </div>

5.接下来，只要运行 `node skeleton.js`，就可以完成骨架屏的注入了。

&emsp;&emsp;可以看到，骨架屏的样式通过 `<style></style>` 标签直接被插入，而骨架屏的内容也被放置在div#app之间。当然，我们还可以进一步处理，把这些内容都压缩一下。改写`skeleton.js`，在里面添加 `html-minifier` 进行压缩。

配置 `skeleton.js` 文件，使用 `html-minifier` 插件

    +   const htmlMinifier = require('html-minifier')
        renderer.renderToString({}, (err, html) => {
    +       html = htmlMinifier.minify(html, {
    +           collapseWhitespace: true,
    +           minifyCSS: true
    +       })
            fs.writeFileSync('index.html', html, 'utf-8')
        })

    cnpm install html-minifier -D

> 可以参考 [Vue页面骨架屏注入实践](https://segmentfault.com/a/1190000014832185/) 这篇文章，本文只是梳理了该博文的内容,标注已安装的模块
## SEO优化
### 服务器端渲染SSR

## 项目优化

由于vue构建的项目越来越大，难以维护，所以前期一定要规划好，严格按照标准进行开发。


### 编写规则

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

### 基础优化

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


7.服务端开启 gzip 压缩

未开启压缩状态
Request Headers里面的Accept-Encoding: gzip只是表示前端（用户浏览器）支持gzip的压缩方式。

开启gzip状态，具体可以看Response Headers中的Content-Encoding字段是否为gzip。

&emsp;&emsp;1.前端需要修改config/index.js文件

    productionGzip: true,    //true为开启Gzip压缩（默认为fals）
    productionGzipExtensions: ['js','css']

&emsp;&emsp;2.前端需要配置build/webpack.prod.conf.js文件

    if(config.build.productionGzip) {
        const CompressionWebpackPlugin = require('compression-webpack-plugin')
        webpackConfig.plugins.push(
            new CompressionWebpackPlugin({
                asset: '[path].gz[query]',
                algorithm: 'gzip',
                test: new RegExp(
                    '\\.(' +
                    config.build.productionGzipExtensions.join('|') +
                    ')$'
                    ),
                threshold: 10240,
                // deleteOriginalAssets:true, //删除源文件，不建议
                minRatio: 0.8
            })
        )
    }


而服务器支持gzip的方式可以有两种：

1.打包的时候生成对应的.gz文件，浏览器请求xx.js时，服务器返回对应的xxx.js.gz文件

2.浏览器请求xx.js时，服务器对xx.js进行gzip压缩后传输给浏览器
## 总结

重点https://juejin.im/entry/58df274f61ff4b006b122948