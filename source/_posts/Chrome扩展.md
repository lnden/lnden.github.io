---
title: Chrome扩展
date: 2018-07-16 22:43:08
tags:
---

[官网](http://airbnb.io/lottie/) | [Github](https://github.com/airbnb/lottie/) | [简书](http://www.jianshu.com/p/9a2136ecbc7b/) | [腾讯云](https://www.qcloud.com/community/article/980116)

### Chrome浏览器扩展程序

&emsp;&emsp;所谓浏览器扩展程序，就是在浏览器的基础上功能上添加一些常用的功能，使浏览器更加强大，并且给工作带来高效！使开发人员得心应手，让用户体验更好。如果你现在还不知道什么是Chrome扩展程序。那我就带你一起进入Chrome扩展程序的世界里进行遨游~

### manifest.json文件

&emsp;&emsp;Chrome扩展程序必须包含一个maniest.json文件，这个文件的文件名固定为manifest.json。Chrome扩展的manifest.json必须包含 `name`、`version` 和 `manifest_version` 属性，目前来说manifest_version属性值只能为数字2，[ 对于Chrome应用来说，还必须包含app属性 ]。

manifest.json
```
    {
        "name": "IDSC 工作助手",
        "version": 1.0.1",
        "manifest_version": 2
        "description": "灵活可配、辅助审核、快捷操作、提能增效。",
        "permissions": [
            "declarativeContent",
            "storage",
            "contextMenus",
            "webRequest",
            "https://asn.qq.com/*",
            "tabs",
            "activeTab",
            "notifications"
        ],
        "background": {
            "scripts": [
                "lib/jquery-3.3.1.js",
                "config.js",
                "backgroundCommon.js",
                "background.js"
            ],
            "persistent": true
        },
        "options_ui": {
            "page": "options.html",
            "open_in_tab": true
        },
        "browser_action": {
            "default_title": "IDSC 工作助手",
            "default_icon": {
                "16": "images/16.png",
                "32": "images/32.png",
                "48": "images/48.png",
                "128": "images/128.png"
            },
            "desc":"1.初始化默认展示login页面",
            "default_popup": "login.html"
        },
        "icons": {
            "16": "images/16.png",
            "32": "images/32.png",
            "48": "images/48.png",
            "128": "images/128.png"
        },
        "content_scripts": [
            {
            "matches": ["https://publicboss.qq.com/*"],
            "css": ["lib/bpoAssistant.css"],
            "js": [
                "lib/jquery-3.3.1.js",
                "lib/jquery-ui-1.12.1/jquery-ui.js",
                "contentFunctionalitySwitch.js",
                "contentCommon.js",
                "contentImageText.js"
            ],
            "run_at": "document_end",
            "all_frames": true
            }
        ],
        "web_accessible_resources": [
            "images/error.jpg",
            "login.html",
            "login.js"
        ]
    }
```
