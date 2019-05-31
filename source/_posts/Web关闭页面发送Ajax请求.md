---
title: Web关闭页面发送Ajax请求
date: 2019-05-09 21:54:59
tags:
---

### 前言

&emsp;&emsp;简单的介绍一下项目需求，后台管系统退出登录的时候要给后台发送请求，所谓的打卡功能。你是不是会想seo easy~ 登出的时候先调一下打卡请求成功之后在调用logout()方法，no no no ! 这才是入坑的开始，正常的思路是规规矩矩的点击退出按钮，但是你要考虑退出系统有很多方式。

例如：

1、右上角的叉号X

2、浏览器页签的小叉号x

3、任务栏右键点击关闭

4、浏览器打开多个页面在重复1和2的步骤

5、强制关机，结束浏览器进程关闭页面

如上这些方式都需要给后台发送打卡请求！

这时候你肯定会想有事件啊，监听浏览器关闭的事件啊，对 没错就是这两个事件`beforeunload`和`unload`事件，下面我们就围绕这两个事件进行分析以及兼容性的处理

### onbeforeunload 事件

- 定义

onbeforeunload 事件在即将离开当前页面(刷新或关闭)时触发。

该事件可以用于弹出对话框，提示用户是继续浏览页面还是离开当前页面。return '是否关闭'对话默认的提示信息根据不同的浏览器有所不同，标准的信息类似"确定要离开此页面吗?"，该信息不能被删除。

- 使用方法

```
window.onbeforeunload = (e) => {
    return '在关闭之前会有提示信息'
}

window.addEventListener("beforeunload", (e) => {
    e.returnValue = '在关闭之前会有提示信息出现'
})
```

用户在当前页面停留时长的业务
```
(function(){
    const startTime = Math.ceil(new Date().getTime()/1000);
    const getDuration = () => {
        let time = '',
            hours = 0,
            minutes = 0,
            seconds = 0,
            endTime = Math.ceil(new Date().getTime()/1000),
            duration = endTime - startTime;
            
        hours = Math.floor(duration/3600);  //停留小时数
        minutes = Math.floor(duration % 3600/60);   //停留分钟数
        seconds = Math.floor(duration % 3600 % 60); //停留秒数
        
        time = (hours < 10 ? '0'+hours : hours) + ':' +
             (minutes < 10 ? '0'+minutes : minutes) + ':' + 
             (seconds < 10 ? '0'+seconds : seconds);
        return time;
    }
    
    window.onbeforeunload = (e) => {
        var duration = getDuration()
        
        //request(duration)
    }
})()

```

### onunload 事件

- 定义

onunload 属性会在页面卸载时触发 (或者浏览器窗口已经关闭)

onunload 在用户从页面导航离开时发生 (通过点击链接。提交表单或者关闭浏览器窗口)

**注释:** 重载页面也会触发 unload 事件

- 使用方法

```
window.onunload = (e) => {
    logout();
}

window.addEventListener("unload", (e) => {
    logout();
})
```

基于以上两个方法就可以实现对页面关闭的时间监听，为了稳妥，可以两个事件都监听。然后对监听函数做处理，让关闭事件只调用一次。

### 请求发送

### H5新特性Beacon API方案

- 使用Blob来发送
&emsp;&emsp;使用blob发送的好处是可以自定义内容的格式和header。比如下面这种设置方式，就可是可以设置content-type为application、x-www-form-urlencoded

```angular2html
    blob = new Blob([`romm_id=123`],{type: 'application/x-www-form-urlencoded'})
    navigator.sendBeacon('/cgi-bin/leave-room',blob)
```
- 使用FormData对象
&emsp;&emsp;但是这时content-type会被设置成"multipart/form-data"
```angular2html
    var fd = new FormData()
    fd.append('room_id',123)
    let result = navigator.sendBeacon('/cgi-bin/leave-room',fd)
    if(result){
        console.log('请求成功排队，等待执行')
    }else{
        console.log('失败')
    }
```

- 数据也可以使用URLSearchParams 对象
&emsp;&emsp;content-type会被设置成"text/plain;charset=UTF-8"
```angular2html
    var params = new URLSearchParams({room_id:123})
    navigator.sendBeacon("/cgi-bin/leave-room",params)
```

虽然现在浏览器对sendBeacon的支持很多，我们对其做一下兼容性处理也是有必要的：
```angular2html
    if(navigator.sendBeacon){
        console.log('执行Beacon代码')
    }else{
        console.log('回退到XHR同步请求或者不做处理')
    }
```