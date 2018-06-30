---
title: wepy框架入门使用及注意事项
date: 2018-02-23 14:16:11
tags:
	- 前端框架
---
[官网](https://tencent.github.io/wepy/index.html) | [NPM](https://www.npmjs.com/package/wepy)  | [Github](https://github.com/Tencent/wepy)

> 简介

&emsp;&emsp;	wepy是微信小程序的官方框架，使用wepy可以快速开发微信小程序，以一个MVVM模式进行开发调试
学习使用wepy框架，你最好是有使用过vue的基础，已经阅读过微信小程序的官网文档最好不过。
&emsp;&emsp;	学习一门新技术我们首先知道为什么要使用它

小程序的缺点
&emsp;&emsp;	不能使用less/sass
&emsp;&emsp;	不能使用npm包及es6高级语法
&emsp;&emsp;	每个页面对应4个文件

wepy 框架的优点
&emsp;&emsp;	针对原生API进行优化
&emsp;&emsp;	单文件模式，统一使用.wpy文件（类似.vue）
&emsp;&emsp;	支持组件化开发
&emsp;&emsp;	支持使用NPM包管理

> wepy创建与使用

&emsp;&emsp;	wepy安装或更新都通过npm包进行管理
1.全局安装或更新wepy命令行工具
```
npm install wepy-cli -g  
```
2.在开发目录中生成工程文件
```
wepy new myproject		//注释1.7以上版本根据提示安装生成 wepy init standard myproject
```
3.切换到项目目录，安装依赖关系
```
cd myproject 	||	npm install
```
4.开启实时编译，生成dist目录
```
wepy build --watch
```
&emsp;&emsp;	此时工程文件已经生成完成并且已经实时监控，使用微信小程序开发者工具打开dist目录查看效果，使用IDE(编辑器) 打开myproject文件进行查看，具体编辑src目录下文件，自动生成dist小程序文件即可。
注释：.wap文件类似.vue文件，一个文件由style、template、script三部分组成。
		
### methods方法使用

	注意，对于WePY中的methods属性，因为与Vue中的使用习惯不一致，非常容易造成误解
	这里需要特别强调一下：WePY中的methods属性只能声明页面wxml标签的bind、catch事件，不能声明自定义方法，这与Vue中的用法是不一致的。
	
### 使用组件的方式

	WePY中，在父组件template模板部分插入驼峰式命名的子组件标签时
	不能将驼峰式命名转换成短横杆式命名(比如将childCom转换成child-com)，这与Vue中的习惯是不一致。
	
### 组件的循环使用
```

<!-- 注意，使用for属性，而不是使用wx:for属性 -->
<repeat for="{{list}}" key="index" index="index" item="item">
	<!-- 插入<script>脚本部分所声明的child组件，同时传入item -->
	<child :item="item"></child>
</repeat>

```

### computed计算属性

```
注意：只要是组件中有任何数据发生了改变，那么所有计算属性就都会被重新计算。
坑  ：任何组件的数据发生了变化都会被监听到，都会被更新

data = {
	a: 1
}

// 计算属性aPlus，在脚本中可通过this.aPlus来引用，在模板中可通过{{ aPlus }}来插值
computed = {
	aPlus () {
		return this.a + 1
	}
}


注释：javascript获取时间戳的方法：
	1、return new Date().getTime();
	2、return +new Date();
	3、return new Date().valueOf();
	4、rfturn Date.parse(new Date());  //获取的时间戳是把毫秒改成000显示，
	
```

### watch监听器


```
data = {
	num: 1
}

// 监听器函数名必须跟需要被监听的data对象中的属性num同名，
// 其参数中的newValue为属性改变后的新值，oldValue为改变前的旧值
watch = {
	num (newValue, oldValue) {
		console.log(`num value: ${oldValue} -> ${newValue}`)
	}
}

// 每当被监听的属性num改变一次，对应的同名监听器函数num()就被自动调用执行一次
onLoad () {
	setInterval(() => {
		this.num++;
		this.$apply();
	}, 1000)
}
```

### wepy自带的一些方法

```
1.在Page页面实例中，可以通过```this.$parent```来访问App实例。
```

### props传值
#### 静态传值
parent.wpy父亲页面
```
<template>
	<child :title="mytitle"></child>
</template>

<script>
	export default class Index extends wepy.page {
		data = {
			mytitle:'我是准备传给儿子组件字符串类型'
		}
	}
</script>
```
child.wpy儿子组件
```
<template>
	<view>{{title}}<view>
</template>

<script>
	export default class Child extends wepy.component {
		props = {
			// 静态传值
			title:String
		}
		onLoad () {
			console.log(this.title); 
		}
	}
</script>
```
-  [x] [shareAll-demo](http://airbnb.io/lottie/)   

#### 动态传值
parent.wpy父亲页面
```
<template>
	<child :title="parentTitle" :syncTitle.sync="parentTitle" :twoWayTitle="parentTitle"></child>
</template>

<script>
	export default class Index extends wepy.page {
		data = {
			parentTitle: 'p-title'
		};
	}
</script>
```
child.wpy儿子组件
```
<template>
	<view>{{title}}<view>
</template>

<script>
	export default class Child extends wepy.component {
		props = {
			// 静态传值
			title: String,

			// 父向子单向动态传值
			syncTitle: {
				type: String,
				default: 'null'
			},

			twoWayTitle: {
				type: Number,
				default: 'nothing',
				twoWay: true
			}
		};
		onLoad () {
			console.log(this.title);	// p-title
			console.log(this.syncTitle);	// p-title
			console.log(this.twoWayTitle);	// p-title

			this.title = 'c-title';
			console.log(this.$parent.parentTitle,111111111111); // p-title.
			this.twoWayTitle = 'two-way-title';
			this.$apply();
			console.log(this.$parent.parentTitle); // two-way-title.  --- twoWay为true时，子组件props中的属性值改变时，会同时改变父组件对应的值
			this.$parent.parentTitle = 'p-title-changed';
			this.$parent.$apply();
			console.log(this.title); // 'c-title';
			console.log(this.syncTitle); // 'p-title-changed' --- 有.sync修饰符的props属性值，当在父组件中改变时，会同时改变子组件对应的值。
		}
	}
</script>
```
-  [x] [shareAll-demo](http://airbnb.io/lottie/)   