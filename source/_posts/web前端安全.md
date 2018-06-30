---
title: web前端安全
date: 2018-03-23 23:39:06
tags:
---
### web前端安全之XSS攻击

#### 简介

&emsp;&emsp;XSS是跨站脚本攻击（Cross-Site Scripting）的简称，XSS这类安全问题发生的本质原因在于，浏览器错误的将攻击者提供的用户输入数据当做JavaScript脚本给执行了。XSS被划分为：

#### 反射型XSS

&emsp;&emsp;数据库中存有的存在XSS攻击的数据，返回给客户端。若数据未经过任何转义，被浏览器渲染，就可能导致XSS攻击。

> 实例：正常发送消息

正常操作

+ <code>http://www.test.com/message.php?send=Hello,World！</code> 接收者将会接收信息并显示 <code>Hello,Word</code>

非正常操作

+ <code>http://www.test.com/message.php?send=&lt;script&gt;alert('foolish!')&lt;/script&gt;！</code> 接收者接收消息显示的时候将会弹出警告窗口

#### 存储型XSS

&emsp;&emsp;将用户输入的存在XSS攻击的数据，发送给后台，后台并未对数据进行存储，也未经过任何过滤，直接返回给客户端。被浏览器渲染。就可能导致XSS攻击；

> 实例：留言板 [ input、textarea ]

正常操作

+ 用户是提交相应留言信息，将数据存储到数据库，其他用户访问留言板时，获取数据并显示。

非正常操作

+ 攻击者在  <code>value</code>  填写 <code>&lt;script&gt;alert('foolish!')&lt;/script&gt;</code> 或者 <code>html</code> 其他标签（破坏样式...）、一段攻击型代码，将数据存储到数据库中，其他用户取出数据显示的时候，将会执行这些攻击性代码。

#### DOMBasedXSS

&emsp;&emsp;当用户能够通过交互修改浏览器页面中的DOM(DocumentObjectModel)并显示在浏览器上时，就有可能产生这种漏洞，从效果上来说它也是反射型XSS。通过修改页面的DOM节点形成的XSS，称之为DOMBasedXSS。

> 实例：页面URL获取参数

正常操作

+ <code>http://www.vulnerable.site/welcome.html?name=Joe</code>

非正操操作

+ <code>http://www.vulnerable.site/welcome.html?name=&lt;script&gt;alert(document.cookie)&lt;/script&gt;</code>

#### XSS危害

&emsp;&emsp;1).通过document.cookie盗取cookie
&emsp;&emsp;2).强制发送电子邮件
&emsp;&emsp;3).盗取各类用户帐号，如机器登录帐号、用户网银帐号、各类管理员帐号
&emsp;&emsp;4).控制企业数据，包括读取、篡改、添加、删除企业敏感数据的能力
&emsp;&emsp;5).控制受害者机器向其它网站发起攻击

#### XSS防御

对cookie的保护
&emsp;&emsp;1).设置httpOnly

对用户输入数据的处理
&emsp;&emsp;1).编码
&emsp;&emsp;2).解码
&emsp;&emsp;3).过滤

### 白帽子讲Web安全

&emsp;&emsp;事情的经过是这样的，自动化测试的同事找过来和我说了一个关于XSS攻击的问题，我当时就在想XSS是什么鬼，这个BUG怎么解决。随后我了解到，XSS就是在用户输入的时候可以输入一个脚本 <code>javascript</code> 执行 <code>alert(1)</code> 也可以输入一个 <code>document.cookie()</code>。并且可以把脚本存储到数据库在反给客户端进行XSS攻击。
&emsp;&emsp;因为这个XSS，引起了我对Web前端安全的研究，机缘巧合我遇见了《白帽子讲Web安全》一书，从而使我对前端安全有一个更多的了解。以下就是我前端安全的一些了解，或者是对本书做一个观后感吧！我会每天持续更新....如果有幸你看见我的bolg，请你关注我，我也会同样的去关注你。让我们一起在github上共同学习，成为前端届的大牛~

+ 纪要：CSDN在2011年12月21日公开道歉，泄漏600万用户数据的数据，并且都是明文显示。对天涯等一些社区也造成用户信息的丢失。