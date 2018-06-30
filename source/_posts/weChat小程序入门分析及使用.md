---
title: weChat小程序入门分析及使用
date: 2018-03-14 10:46:32
tags:
	- 前端框架  
---
### 微信小程序

> 相关地址

[小程序开发文档](https://mp.weixin.qq.com/debug/wxadoc/dev/index.html)

> 前言

&emsp;&emsp;这是小程序入门基础篇，要学习一门新的技术首先要对它整体有一个简单的了解。微信小程序是寄生在微信App里面，微信拥有的功能大部分对小程序是开放的，可以在小程序里面任意调用微信原生API。开发者工具的使用以及安装请参考官方文档进行安装。
&emsp;&emsp;如果你想快速掌握小程序，下面你就应该走走心了！我主要从下面几个方面介绍。

一、小程序的文件结构：
&emsp;&emsp;小程序主要由三个构造器函数生成，<code>App()</code> 函数用来注册小程序、<code>Page()</code> 函数用来注册页面、<code>Component()</code> 函可用来定义组件。一个小程序只需要有一个 <code>App()</code> 函数，可以有一个或者多个 <code>Page()</code> 和 <code>Component()</code> 函数。

二、一个小程序主体部分由三个文件组成，必须放在项目的根目录，如下：
&emsp;&emsp;<code>app.js</code> 小程序主逻辑生命周期、首次授权验证、<code>app.json</code> 小程序公共设置、<code>app.wxss</code> 小程序公共样式表。

三、一个小程序页面由四个文件组成，分别是：
&emsp;&emsp;<code>.js</code> 页面逻辑、<code>.wxml</code> 页面结构、<code>.wxss</code> 页面样式表、<code>.json</code> 页面配置。
这里我已经把小程序里面主要文件以及组成罗列出来了，下面我们就对应各类型的文件来详细讲解。

### 配置

> #### app.json配置信息

<code>app.json</code> 文件用来对微信小程序进行全局配置，决定页面文件的路径、窗口表现、设置网络超时时间、设置多 tab 等。
```
	{		
	  "pages": [
		"pages/index/index",
		"pages/logs/index"
	  ],
	  "window": {
		"navigationBarTitleText": "Demo"
	  },
	  "tabBar": {
		"list": [{
		  "pagePath": "pages/index/index",
		  "text": "首页"
		}, {
		  "pagePath": "pages/logs/logs",
		  "text": "日志"
		}]
	  },
	  "networkTimeout": {
		"request": 10000,
		"downloadFile": 10000
	  },
	  "debug": true
	}
```
pages	<font color="#aaa"><em>设置页面路径</em></font>
window	<font color="#aaa"><em>设置默认页面的窗口表现</em></font>
tabBar	<font color="#aaa"><em>设置底部 tab 的表现</em></font>
networkTimeout	<font color="#aaa"><em>设置网络超时时间</em></font>
debug	<font color="#aaa"><em>设置是否开启 debug 模式</em></font>

<font color="#f00"><strong>注意：</strong></font> 上面是小程序的全局配置信息，该配置也可以用在各页面的.json文件内配置局部信息

### 逻辑层

> #### 注册程序App()

<code>App()</code> 函数用来注册一个小程序。接受一个 <code>object</code> 参数，其指定小程序的生命周期函数等。(app.js文件)
```
	App({
	  //生命周期函数--监听小程序初始化
	  onLaunch: function(options) {
		  // 当小程序初始化完成时，会触发 onLaunch（全局只触发一次）
	  },
	  //生命周期函数--监听小程序显示	
	  onShow: function(options) {
		  // 当小程序启动，或从后台进入前台显示，会触发 onShow
	  },
	  //生命周期函数--监听小程序隐藏
	  onHide: function() {
		  // 当小程序从前台进入后台，会触发 onHide
	  },
	  onError: function(msg) {
		  // 当小程序发生脚本错误，或者 api 调用失败时，会触发 onError 并带上错误信息
	  },
	  globalData: 'I am global data'
	})
```
<font color="#f00"><strong>注意：</strong></font> 小程序应用生命周期有三个:初始化、显示、隐藏

> #### 注册页面Page()

<code>Page()</code> 函数用来注册一个页面。接受一个 <code>object</code> 参数，其指定页面的初始数据、生命周期函数、事件处理函数等(index.js文件)
      
```
	//index.js
	Page({
	  data: {
		text: "This is page data."
	  },
	  onLoad: function(options) {
		// Do some initialize when page load.
	  },
	  onReady: function() {
		// Do something when page ready.
	  },
	  onShow: function() {
		// Do something when page show.
	  },
	  onHide: function() {
		// Do something when page hide.
	  },
	  onUnload: function() {
		// Do something when page close.
	  },
	  onPullDownRefresh: function() {
		// Do something when pull down.
	  },
	  onReachBottom: function() {
		// Do something when page reach bottom.
	  },
	  onShareAppMessage: function () {
	  // return custom share data when user share.
	  },
	  onPageScroll: function() {
		// Do something when page scroll
	  },
	  onTabItemTap(item) {
		console.log(item.index)
		console.log(item.pagePath)
		console.log(item.text)
	  },
	  // Event handler.
	  viewTap: function() {
		this.setData({
		  text: 'Set some data for updating view.'
		}, function() {
		  // this is setData callback
		})
	  },
	  customData: {
		hi: 'MINA'
	  }
	})
```
一、页面生命周期

<code>onLoad</code>: 页面加载
&emsp;&emsp;一个页面只会调用一次，可以在 onLoad 中获取打开当前页面所调用的 query 参数。
<code>onShow</code>: 页面显示
&emsp;&emsp;每次打开页面都会调用一次。
<code>onReady</code>: 页面初次渲染完成
&emsp;&emsp;一个页面只会调用一次，代表页面已经准备妥当，可以和视图层进行交互。对界面的设置如wx.setNavigationBarTitle请在onReady之后设置。详见生命周期
<code>onHide</code>: 页面隐藏
&emsp;&emsp;当navigateTo或底部tab切换时调用。
<code>onUnload</code>: 页面卸载
&emsp;&emsp;当redirectTo或navigateBack的时候调用。

   
二、页面相关事件处理函数

<code>onPullDownRefresh</code>: 下拉刷新
&emsp;&emsp;监听用户下拉刷新事件。需要在app.json的window选项中或页面配置中开启enablePullDownRefresh。当处理完数据刷新后，wx.stopPullDownRefresh可以停止当前页面的下拉刷新。
<code>onReachBottom</code>: 上拉触底
&emsp;&emsp;监听用户上拉触底事件。可以在app.json的window选项中或页面配置中设置触发距离onReachBottomDistance。在触发距离内滑动期间，本事件只会被触发一次。
<code>onPageScroll</code>: 页面滚动
&emsp;&emsp;监听用户滑动页面事件。参数为 Object，包含以下字段：
<code>onShareAppMessage</code>: 用户转发
&emsp;&emsp;只有定义了此事件处理函数，右上角菜单才会显示“转发”按钮。用户点击转发按钮的时候会调用，此事件需要 return 一个 Object，用于自定义转发内容｛title:"转发标题"， path:"转发路径"｝
    
转发示例代码
```
	Page({
	  onShareAppMessage: function () {
		return {
		  title: '自定义转发标题',
		  path: '/page/user?id=123'
		}
	  }
	})
```

事件处理函数

```
	<view bindtap="viewTap">click me</view>
```
```
	Page({
		viewTap:function(){
			console.log('view tap');
		}
	})
```
数据绑定  
```
	<view>{{text}}</view>
```
```
	Page({
	  data: {
		text: 'init data',
		num: 0,
		array: [{text: 'init data'}],
		object: {
		  text: 'init data'
		}
	  }
	  changeText:function(){
		  this.setData({
			  text:'change data'
		  })
	  }
	})
```
    
getApp()

全局的 <code>getApp()</code> 函数可以用来获取到小程序实例。
  
```
	// other.js
	var appInstance = getApp()
	console.log(appInstance.globalData) // I am global data
```
> #### 注册自定义组件Component()

&emsp;&emsp;从小程序基础库版本 1.6.3 开始，小程序支持简洁的组件化编程。所有自定义组件相关特性都需要基础库版本 1.6.3 或更高。
开发者可以将页面内的功能模块抽象成自定义组件，以便在不同的页面中重复使用；也可以将复杂的页面拆分成多个低耦合的模块，有助于代码维护。自定义组件在使用时与基础组件非常相似。

##### 自定义组件的创建与使用

&emsp;&emsp;类似于 <code>Page()</code> 页面，一个自定义组件由 .json、.wxml、.wxss、.js 4个文件组成，下面我们看下.js文件内容
 <code>Component</code> 构造器可用于定义组件，调用 <code>Component</code> 构造器时可以指定组件的属性、数据、方法等。

```
	Component({
		options:{},
		externalClasses: ['my-class'],
		properties:{
			myProperty:{}
		},
		data:{},
		methods:{
			_myPrivateMethod: function(){}  //内部方法建议以下划线开头
		}
	})
```
1). 要编写一个自定义组件，首先需要在新组件.json文件中进行自定义组件声明将 <code>component</code> 字段设为 <code>true</code>
```
	{
	  "component": true
	}
```
2). 编写自定义组件的 <code>wxml</code> 模板和 <code>wxss</code> 样式
```
	<!-- 这是自定义组件的内部WXML结构 -->
	<view class="inner">
	  {{innerText}}
	</view>
	<!-- 占位符可以不写，暂时用不上  -->
	<slot></slot>
	/* 这里的样式只应用于这个自定义组件 */
	.inner {
	  color: red;
	}
```
3). 编写自定义组件的js部分内容，需要使用 <code>Component()</code> 实例来注册

```
	Component({
	 options: {
		// 在组件定义时的选项中启用多slot支持
		multipleSlots: true 
	  },
	  properties: {
		// 这里定义了innerText属性，属性值可以在组件使用时指定
		innerText: {
		  type: String,
		  value: 'default value',
		}
	  },
	  data: {
		// 这里是一些组件内部数据
		someData: {}
	  },
	  methods: {
		// 这里是一个自定义方法
		customMethod: function(){}
	  }
	})
```
4). 使用自定义组件

使用已注册的自定义组件，首先要在[使用页]json文件中引用声明。此时需要提供每个自定义组件的标签名和对应的自定义组件文件路径：
```
	{
	  "usingComponents": {
		<!-- 组件的名称标签名 : 组件的路径 -->
		"component-tag-name": "/components/Banner/banner"
	  }
	}
```
现在可以在使用的页面 <code>wxml</code> 中就可以像使用基础组件一样使用自定义组件了。
```
	<view>
	  <!-- 以下是对一个自定义组件的引用 [inner-text='some text'] 属性不写的时候默认展示组件js中properties的value值-->
	  <component-tag-name inner-text="Some text"></component-tag-name>
	</view>
```


##### 组件的模板和样式

1.组件模板 

在组件模板中可以提供一个 <slot> 节点，用于承载组件引用时提供的子节点。
```
	<!-- 组件模板 -->
	<view class="wrapper">
	  <view>这里是组件的内部节点</view>
	  <slot></slot>
	</view>
```
```
	<!-- 引用组件的页面模版 -->
	<view>
	  <component-tag-name>
		<!-- 这部分内容将被放置在组件 <slot> 的位置上 -->
		<view>这里是插入到组件slot中的内容</view>
	  </component-tag-name>
	</view>
```
组件 <code>wxml</code> 的 <code>slot</code> 使用

在组件的 <code>wxml</code> 中可以包含 <code>slot</code> 节点，用于承载组件使用者提供的 <code>wxml</code> 结构。

默认情况下，一个组件的wxml中只能有一个 <code>slot</code> 需要使用多 <code>slot</code> 时，可以在组件js中声明启用。
```
	Component({
	  options: {
		multipleSlots: true // 在组件定义时的选项中启用多slot支持
	  },
	  properties: { /* ... */ },
	  methods: { /* ... */ }
	})
```
此时，可以在这个组件的 <code>wxml</code> 中使用多个 <code>slot</code> 以不同的 <code>name</code> 来区分。
```
	<!-- 组件模板 -->
	<view class="wrapper">
	  <slot name="before"></slot>
	  <view>这里是组件的内部细节</view>
	  <slot name="after"></slot>
	</view>
```
使用时，用 <code>slot</code> 属性来将节点插入到不同的 <code>slot</code> 上。
```
	<!-- 引用组件的页面模版 -->
	<view>
	  <component-tag-name>
		<!-- 这部分内容将被放置在组件 <slot name="before"> 的位置上 -->
		<view slot="before">这里是插入到组件slot name="before"中的内容</view>
		<!-- 这部分内容将被放置在组件 <slot name="after"> 的位置上 -->
		<view slot="after">这里是插入到组件slot name="after"中的内容</view>
	  </component-tag-name>
	</view>
```
2.组件的样式
外部样式类（注意：1.9.90以上版本才支持）
```
	/* 组件页面 custom-component.js */
	Component({
	  externalClasses: ['my-class']
	})

	<!-- 组件 custom-component.wxml -->
	<custom-component class="my-class">这段文本的颜色由组件外的 class 决定</custom-component>
	
	<!-- 引用页面的 WXML -->
	<custom-component my-class="red-text" />
	
	/* 引用页面的样式 */
	.red-text {
	  color: red;
	}
```

##### 组件事件

&emsp;&emsp;事件系统是组件间交互的主要形式。自定义组件可以出发任意事件，引用组件的页面可以监听这些事件。   
```
	<!-- 当自定义组件触发“myevent”事件时，调用“onMyEvent”方法 -->
	<component-tag-name bindmyevent="onMyEvent" />
	<!-- 或者可以写成 -->
	<component-tag-name bind:myevent="onMyEvent" />
	```
	```
	<!-- 当前引用页面 -->
	Page({
	  onMyEvent: function(e){
		console.log(e.detail);   // 自定义组件触发事件时提供的detail对象
	  }
	})
```

自定义组件触发事件时，需要使用 <code>triggerEvent</code> 方法，指定事件名 <code>detail</code> 对象和事件选项：

```
	<!-- 在自定义组件中 -->
	<button bindtap="onTap">点击这个按钮将触发“myevent”事件</button>
```
```
	Component({
	  properties: {}
	  methods: {
		onTap: function(){
		  var myEventDetail = {} // detail对象，提供给事件监听函数
		  var myEventOption = {} // 触发事件的选项
		  this.triggerEvent('myevent', myEventDetail, myEventOption)
		}
	  }
	})
```
> #### 页面路由

<code>getCurrentPages()</code> 函数用于获取当前页面栈的实例，以数组形式按栈的顺序给出。

第一个元素为首页，最后一个元素为当前页面

路由的方式

&emsp;&emsp;打开新页面调用 wx.navigateTo 或使用组件 &lt;navigator open-tpe='navigateTo' /&gt;

&emsp;&emsp;页面重定向调用 wx.redirectTo 或使用组件 &lt;navigator open-type='redirectTo' /&gt;

&emsp;&emsp;页面返回调用 wx.navigateBack 或使用组件 &lt;navigator open-type='navigateBack'&gt; 或者左上角返回按钮

&emsp;&emsp;Tab切换调用 wx.switchTab 或使用组件 &lt;navigator open-type='switchTab'/&gt;

&emsp;&emsp;重启动调用 wx.relaunch 或使用组件 &lt;navigator open-type='relaunch' /&gt;

> #### 模块化

可以将一些公共的代码抽离成为一个单独的js文件，作为一个模块。模块只有通过 <code>module.export</code> 或者 <code>export</code> 才能对外暴露接口。

```
	//comm.js
	function sayHello(name){
		console.log(`hello ${name} !`)
	}
	function sayGoodbye(name){
		console.log(`Goodbye ${name} !`)
	}
	module.export.sayHello = sayHello
	export.sayGoodbye = sayGoodbye
```

在需要使用这些模块的文件中，使用 <code>require(path)</code> 将公共代码引入
```
	var comm = require('comm.js')           //注：path暂不支持绝对路径
	Page({
		helloMINA:function(){
			comm.sayHello('MINA')
		},
		goodbyeMINA:function(){
			comm.sayGoodbye('MINA')
		}
	})
```

> #### 小程序API

&emsp;&emsp;小程序提供丰富的微信原生API，可以方便调起微信提供的能力，用户信息、本地存储、支付功能、网络请求（    只有你想不到的,没有它做不到的。）

&emsp;&emsp;需要参考官方API文档[微信小程序官方API](https://mp.weixin.qq.com/debug/wxadoc/dev/api/)

### 视图层

> #### WXML

WXML（WeiXin Markup Language）是框架设计的一套标签语言，结合基础组件、事件系统，可以构建出页面的结构。

数据绑定

```
	<!--wxml-->
	<view>{{message}}</view>
```
```
	//page.js
	Page({
		data:{
			message:'Hello MINA!'
		}
	})
```
列表渲染
```
	<!--wxml-->
	<view wx:for="{{array}}"> {{item}} </view>
```
```
	// page.js
	Page({
	  data: {
		array: [1, 2, 3, 4, 5]
	  }
	})
```
条件渲染
```
	<!--wxml-->
	<view wx:if="{{view == 'WEBVIEW'}}"> WEBVIEW </view>
	<view wx:elif="{{view == 'APP'}}"> APP </view>
	<view wx:else="{{view == 'MINA'}}"> MINA </view>
```
```
	// page.js
	Page({
	  data: {
		view: 'MINA'
	  }
	})
```
模板
```
	<!--wxml-->
	<template name="staffName">
	  <view>
		FirstName: {{firstName}}, LastName: {{lastName}}
	  </view>
	</template>

	<template is="staffName" data="{{...staffA}}"></template>
	<template is="staffName" data="{{...staffB}}"></template>
	<template is="staffName" data="{{...staffC}}"></template>
```
```
	// page.js
	Page({
	  data: {
		staffA: {firstName: 'Hulk', lastName: 'Hu'},
		staffB: {firstName: 'Shang', lastName: 'You'},
		staffC: {firstName: 'Gideon', lastName: 'Lin'}
	  }
	})
```

> 事件

```
	<!--wxml-->
	<view bindtap="add"> {{count}} </view>

	// page.js
	Page({
	  data: {
		count: 1
	  },
	  add: function(e) {
		this.setData({
		  count: this.data.count + 1
		})
	  }
	})
```
> #### WXS

> #### WXSS

WXSS(WeiXin Style Sheets)是一套样式语言，用于描述 WXML 的组件样式。

##### 尺寸单位

&emsp;&emsp;如在 iPhone6 上，屏幕宽度为375px，共有750个物理像素，则750rpx = 375px = 750物理像素，1rpx = 0.5px = 1物理像素。

##### 样式导入

&emsp;&emsp;使用 <code>@import</code> 语句可以导入外联样式表，<code>@import</code> 后跟需要导入的外联样式表的相对路径，用;表示语句结束。
```
	/** common.wxss **/
	.small-p {
	  padding:5px;
	}
```
```
	/** app.wxss **/
	@import "common.wxss";
	.middle-p {
	  padding:15px;
	}
```
##### 内联样式

&emsp;&emsp;框架组件上支持使用 <code>style</code>、<code>class</code> 属性来控制组件的样式。
```
	/** style请尽量避免将静态的样式写进 style 中，以免影响渲染速度。 **/
	<view style="color:{{color}};" />
	
	/** class 使用要遵守命名规范 **/
	<view class="normal_view" />
```

定义在 <code>app.wxss</code> 中的样式为全局样式，作用于每一个页面。在 <code>page</code> 的 <code>wxss</code> 文件中定义的样式为局部样式，只作用在对应的页面，并会覆盖 <code>app.wxss</code> 中相同的选择器。

### 插件

2018/3/13推送插件启用，目前只是企业账号才可以使用。

### 实战常用的功能

#### 小程序登陆授权验证
&emsp;&emsp;首先登陆授权主要是获取openid，这个值可以在前端获取也可以在后台获取，官方推荐的方式是后台获取，不对外提供。
后台获取：
1.小程序调用wx.login() 获取 临时登录凭证code ，并回传到开发者服务器
2.开发者服务器以code换取和appid + appsecret用户唯一标识openid 和 会话密钥session_key。

或者是

1.获取code(登录凭证，用来换取openid及session_key等)
```
wx.login({
    success:  function(res){
        if(res.code){
            that.getEncData(res.code);
        }else{
            console.log('获取用户登录态失败！'+res.errMsg);
        }
    }
})
```
2.获取加密数据和加密算法初始向量
```
getEncData:  function(code){
    wx.getUserInfo({
        withCredentials: true，
        success: function(res){
            that.getNeededUserInfo( code,  res.encryptedData,  res.encryptedData );
        }
    })
}
```
3. 获取用户信息（利用wx.login返回的code获取用户的信息）

```
getNeededUserInfo:  function(code, enc, iv){
    wx.request({
        url: 'https://my.com/login',
        method: 'POST',
        data: {
            code: code,
            encryptedData: enc,
            iv: iv
        },
        success: function(res){
            // 可以返回前端需要的用户信息（包括unionid、openid、user_id等）
        }
    })
}
```

前端获取：
1.首先你需要取微信小程序的后台配置请求的地址<code>https://api.weixin.qq.com</code>
2.之后去微信小程序后台开发设置里面拿到AppID 和 AppSecret
注释：AppSecret这个小程序密钥不会轻易出现在前端页面，所以官方不推荐前端获取openid的方式。

```
wx.login({
    success: function(loginRes) {
        if (loginRes.code) {
            //调用系统验证，在向微信获取openid
            wx.getUserInfo({
                success:function(res){
                    var APPID = 'wxaeb208664e642xxx';
                    var SECRET = '1275d4508ae8d611c9ba026ac01fxxx';
                    wx.request({
                        url: 'https://api.weixin.qq.com/sns/jscode2session?appid='+APPID+'&secret='+SECRET+'&js_code='+loginRes.code+'&grant_type=authorization_code',
                        success:function(response){
                            console.log('openid:' + response.data.openid);
                        },
                    })
                }
            })
        }
    }
});
```
[授权登陆官方文档]('https://developers.weixin.qq.com/miniprogram/dev/api/open.html#wxgetuserinfoobject')
[博客介绍]('https://www.cnblogs.com/yaoyuqian/p/8203792.html')
2018年5月10日 星期四 23:02 <font color="#f00">《获取用户信息接口优化调整》</font>
&emsp;&emsp;此次优化调整，wx.getUserInfo 无法调起微信授权弹框，如果需要调起弹框需要使用button组件拉起，已经上线的项目暂不受影响。


#### 禁止屏幕上下滑动以及下拉上拉执行的方法

具体页面的.json文件：
```
	{
		"enablePullDownRefresh": true	//页面下拉是否触发滑动事件
		"onReachBottomDistance":Number	//页面上拉与页面底部的距离
	}
```
app.json文件
```
	"window": {
		"enablePullDownRefresh": true
	}
```
下拉刷新回调接口
```
	onPullDownRefresh: function () {
		// do somthing matter
	},
```
上拉加载回调接口
```
	onReachBottom: function () {
		// do somthing matter
	},
```
#### 固定屏幕

只显示一屏内容，不可上下滑动,值允许在 <code>page.json</code> 中设置，无法在 <code>app.json</code> 中设置
```
	{
		"disableScroll":true
	}
```
#### 实现点击复制指定内容
```
	clone(){
		wx.setClipboardData({
			data: '123456',
			success: function(res) {
				wx.getClipboardData({
					success: function(res) {
						console.log(res.data); // data
						wx.showToast({
							title: '复制成功',
						})
					}
				})
			}
		})
	}
```

#### 实现双击
```
	clickBtn(e){
		var that = this;
		// 控制点击事件在350ms内触发，加这层判断是为了防止长按时会触发点击事件
		if (that.touchEndTime - that.touchStartTime < 350) {
			// 当前点击的时间
			var currentTime = e.timeStamp;
			var lastTapTime = that.lastTapTime;
			// 更新最后一次点击时间
			that.lastTapTime = currentTime;
			// 如果两次点击时间在300毫秒内，则认为是双击事件
			if (currentTime - lastTapTime < 300) {
				if(this.flag){
					this.videoContext.play();
					this.flag = false;
				}else{
					this.videoContext.pause();
					this.flag = true;
				}
				wx.showModal({
				  title: '提示',
				  content: '双击事件被触发',
				  showCancel: false
				})
			}
		}
	}
```
#### 调用客服API

第一种实现方式：

```
<view class='myView'>第一种方式实现：</view>
<contact-button class='cBtn' type="default-dark" size="20" session-from="weapp">
</contact-button>
```
第二种实现方式：
```
<view class='myView'>第二种方式实现：</view>
 <button class="cs_button" open-type="contact" session-from="weapp">
  <image class="cs_image" src="../images/cs.png"></image>
</button> 
```

#### 用户授权拒绝
需求：
&emsp;&emsp;1).当用户首次进入小程序点击允许可以继续访问页面，点击拒绝直接退出小程序
```
wx.navigateBack({
    delta: -1
})
//-1可以直接强退小程序，但再次进入的时候无法调起授权弹框
```

&emsp;&emsp;2).当用户首次进入小程序点击允许可以继续访问页面，点击拒绝直跳转到新页显示授权按钮
在新页面使用一个按钮绑定属性和事件来调用是否授权提示框
```
<button open-type="getUserInfo"  
        class="recordingNoneS wx-contact-text" 
        @getuserinfo="userInfoHandler">点击授权</button>

userInfoHandler(e){
    if(e && e.mp && e.mp.detail && e.mp.detail.encryptedData){
        wx.setStorageSync('nickName', e.mp.detail.userInfo.nickName);
        wx.setStorageSync('avatarUrl', e.mp.detail.userInfo.avatarUrl);
        this.getVerifyInfo(e.mp.detail.iv,e.mp.detail.encryptedData);
        console.log(e.mp.detail);
    }
},
```
注：如果用户拒绝授权还可以引导手动开启授权
```
wx.openSetting({
    success:(res)=>{
        console.log(res);
    }
});
```
[官方回复](https://developers.weixin.qq.com/blogdetail?action=get_post_info&lang=zh_CN&token=1575921750&docid=c45683ebfa39ce8fe71def0631fad26b)

#### 分享功能两种实现方式

启用右上角分享
```
Page({
    onShareAppMessage: function (res) {
        title: '自定义转发标题',
        path: '/page/user?id=123',
        success: function(res) {
            // 转发成功
        },
        fail: function(res) {
            // 转发失败
        }
    }
})
```
启用按钮分享
```
<button open-type="share">转发</button>
```
混合使用
```
onShareAppMessage: function( options ){
　　var that = this;
　　// 设置菜单中的转发按钮触发转发事件时的转发内容
　　var shareObj = {
　　　　title: "转发的标题",        // 默认是小程序的名称(可以写slogan等)
　　　　path: '/pages/share/share',        // 默认是当前页面，必须是以‘/’开头的完整路径
　　　　imgUrl: '',     //自定义图片路径，可以是本地文件路径、代码包文件路径或者网络图片路径，支持PNG及JPG，不传入 imageUrl 则使用默认截图。显示图片长宽比是 5:4
　　　　success: function(res){
　　　　　　// 转发成功之后的回调
　　　　　　if(res.errMsg == 'shareAppMessage:ok'){
　　　　　　}
　　　　},
　　　　fail: function(){
　　　　　　// 转发失败之后的回调
　　　　　　if(res.errMsg == 'shareAppMessage:fail cancel'){
　　　　　　　　// 用户取消转发
　　　　　　}else if(res.errMsg == 'shareAppMessage:fail'){
　　　　　　　　// 转发失败，其中 detail message 为详细失败信息
　　　　　　}
　　　　},
　　　　complete: fucntion(){
　　　　　　// 转发结束之后的回调（转发成不成功都会执行）
　　　　}
　　};
　　// 来自页面内的按钮的转发
　　if( options.from == 'button' ){
　　　　var eData = options.target.dataset;
　　　　console.log( eData.name );     // shareBtn
　　　　// 此处可以修改 shareObj 中的内容
　　　　shareObj.path = '/pages/btnname/btnname?btn_name='+eData.name;
　　}
　　// 返回shareObj
　　return shareObj;
}
```
注意：使用mpvue的时候默认所有页面均带分享功能，官方暂时没有给出解决方案。但是...但是...这里有一个解决方案
&emsp;&emsp;使用mpvue所有页面全部都有分享功能使用onShareAppMessage初始化一下分享，这时候原生小程序，覆盖掉了mpvue的分享，在onLoad的时候添加wx.hideShareMenu();方法，这时候达到的效果：点击右上角，不会出现转发项。

#### 禁止下滑与页面堆栈
1.ios不可以禁止下滑，可以使用view-scroll，进行一点优化
2.重复路径来回跳转，栈堆积到5-8层左右，禁止任何点击，栈堆满了，无法解决
### 总结

&emsp;&emsp;我已经把小程序的框架简单的介绍了一遍，希望在脑海里有一个清晰的思路，这样运用起来将会非常得心应手。小程序还有两部分内容，一部分是小程序自带的一些组件，另一部分是小程序的丰富的微信原生API，这两部分内容就没什么可说的了，需要使用什么组件或者是需要调用什么API自己去文档查吧！very easy
&emsp;&emsp;下面奉上两个地址：[官方组件地址](https://mp.weixin.qq.com/debug/wxadoc/dev/component/)，[官方API地址](https://mp.weixin.qq.com/debug/wxadoc/dev/api/)，前端的你时时刻刻都需要提升自己































