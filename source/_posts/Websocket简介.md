---
title: WebSocket简介
date: 2018-11-16 09:47:20
tags:
---

### 前言

&emsp;&emsp;在前端 提起 WebSocket 你可能会联想到聊天室这样实时通信的应用。一副高大上的样子。面试的时候面试官问你知道 WebSocket 吗？又是一脸懵逼的样子，项目中没使用过。但是我自己了解过，是一个可以主动向客户端发起请求的一个H5协议。没关系！谁还不是从[新人]=>[大牛]一步一步走过来的。如果你对WebSocket还有疑惑让我们来一起学习它！

> 学习一门技术，你一定认真的思考下面几点：
&emsp;&emsp;1.它是干什么的？
&emsp;&emsp;2.它的优点是什么？
&emsp;&emsp;3.它的缺点是什么？

我们先来了解一下WebSocket及它的优缺点
&emsp;&emsp;WebSocket协议是基于TCP的一种新的网络协议。它实现了浏览器与服务器全双工(full-duplex)通信——允许服务器主动发送信息给客户端。(说白了就是没出现WebSocket之前，服务端是没办法主动给客户端发起通信的，只能由客户端定时向服务端发起请求。)
&emsp;&emsp;WebSocket可以节约带宽，无浪费资源。不停地轮询服务端数据这种方式，使用的是http协议，head信息很大，有效数据占比低， 而使用WebSocket方式，头信息很小，有效数据占比高。轮询方式有可能轮询10次，才碰到服务端数据更新，那么前9次都白轮询了，因为没有拿到变化的数据。 而WebSocket是由服务器主动回发，来的都是新数据。
&emsp;&emsp;WebSocket缺点也很明显，它对开发者要求高了很多，对前端开发者，往往要具备数据驱动使用javascript的能力，且需要维持住ws连接（否则消息无法推送）

### 为什么会出现WebSocket?

&emsp;&emsp;HTTP 协议是一种无状态的、无连接的、单向的应用层协议。它采用了请求/响应模型。通信请求只能由客户端发起，服务端对请求做出应答处理。
这种通信模型有一个弊端：HTTP 协议无法实现服务器主动向客户端发起消息。
&emsp;&emsp;这种单向请求的特点，注定了如果服务器有连续的状态变化，客户端要获知就非常麻烦。大多数 Web 应用程序将通过频繁的异步JavaScript和XML（AJAX）请求实现长轮询。轮询的效率低，非常浪费资源（因为必须不停连接，或者 HTTP 连接始终打开）。因此，工程师们一直在思考，有没有更好的方法。WebSocket 就是这样发明的。WebSocket 连接允许客户端和服务器之间进行全双工通信，以便任一方都可以通过建立的连接将数据推送到另一端。WebSocket 只需要建立一次连接，就可以一直保持连接状态。这相比于轮询方式的不停建立连接显然效率要大大提高。

啰嗦了这么多，就是想让大家对 WebSocket 有一个大概的了解，下面让我来看看 WebSocket API 。

### WebSocket 客户端API


#### 创建一个WebSocket对象

    const Socket = new WebSocket(url[,protocol])

第一个参数 url, 指定连接的 URL。第二个参数 protocol 是可选的，指定了可接受的子协议。

#### WebSocket属性

属性 | 描述
---|---
Socket.readState | 只读属性 readyState 表示连接状态，可以是以下值：<br>0 - 表示连接尚未建立。<br>1 - 表示连接已建立，可以进行通信。<br>2 - 表示连接正在进行关闭。<br>3 - 表示连接已经关闭或者连接不能打开。
Socket.bufferedAmount | 只读属性 bufferedAmount 已被 send() 放入正在队列中等待传输，但是还没有发出的 UTF-8 文本字节数。


#### WebSocket方法

方法 | 描述
---|---
Socket.send() | 使用连接发送数据
Socket.close() | 关闭连接

#### WebSocket事件

事件 | 事件处理方式 | 描述
---|---|---
open | Socket.onopen() = () => | 连接建立时触发
message |  Socket.onmessage() = () => | 客户端接收服务端数据时触发
error |  Socket.onerror() = () => | 通信发生错误时触发
close |  Socket.onclose() = () => | 连接关闭时触发


### Example

1.属性、方法、事件应用

    var ws = new WebSocket("wss://echo.websocket.org");

    ws.onopen = function(evt) {
        console.log("Connection open ...");
        ws.send("Hello WebSockets!");
    };

    ws.onmessage = function(evt) {
        console.log( "Received Message: " + evt.data);
        ws.close();
    };

    ws.onclose = function(evt) {
        console.log("Connection closed.");
    };

    ws.onerror = function(evt) {
        console.log("websocket is error.");
    };

2.提升应用

    ws.onopen = function(evt) {
        console.log("Connection open ...");
        ws.send("Hello WebSockets!");

        //发送 Blob 对象的例子。
        var file = document.querySelector('input[type="file"]').files[0];
        ws.send(file);

        //发送 ArrayBuffer 对象的例子。
        var img = canvas_context.getImageData(0, 0, 400, 320);
        var binary = new Uint8Array(img.data.length);
        for (var i = 0; i < img.data.length; i++) {
            binary[i] = img.data[i];
        }
        ws.send(binary.buffer);
    };

    ws.onmessage = function(evt) {
        //获取信息可能是文本，也可能是二进制数据（blob对象或Arraybuffer对象）。
        if(typeof event.data === String) {
            console.log("Received data string");
        }

        if(event.data instanceof ArrayBuffer){
            var buffer = event.data;
            console.log("Received arraybuffer");
        }

        //除了动态判断收到的数据类型，也可以使用binaryType属性，显式指定收到的二进制数据类型。
        // 收到的是 blob 数据
        ws.binaryType = "blob";
        ws.onmessage = function(e) {
            console.log(e.data.size);
        };

        // 收到的是 ArrayBuffer 数据
        ws.binaryType = "arraybuffer";
        ws.onmessage = function(e) {
            console.log(e.data.byteLength);
        };
    };

**资源链接：**
&emsp;&emsp;[阮一峰WebSocket教程](http://www.ruanyifeng.com/blog/2017/05/websocket.html)&emsp;[静默虚空博客](https://www.cnblogs.com/jingmoxukong/p/7755643.html)