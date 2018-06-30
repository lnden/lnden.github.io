---
title: bodymovin动画使用
date: 2018-02-01 13:50:08
tags:
---

[官网](http://airbnb.io/lottie/) | [Github](https://github.com/airbnb/lottie/) | [简书](http://www.jianshu.com/p/9a2136ecbc7b/) | [腾讯云](https://www.qcloud.com/community/article/980116)

### jQuery 使用 bodymovin 方法

>   1.首先你要要引入bodymovin.js库(此插件不依赖其他库,可以使用CDN和目录文件)

     <script src="https://cdn.bootcss.com/bodymovin/4.13.0/bodymovin.min.js"></script>
     <script  type="text/javascript" src="js/bodymovin.js"></script>
     
>   2.需要添加展示动画的标签div

     <a href="javascript:;">
        <figure>
            <img style="display: block" src="data:image/png;base64..."/>
            <div style="display: none" id="bm" class="bodymovin" ></div>
        </figure>
        <figcaption>
            发现
        </figcaption>
    </a>   
    
>   3.页面初始化加载完成之后执行showRecommen()

```
    showRecommend:function(){
        if(!TempCache.getItem('isFirst')){
            $('.al-footerBarItem:nth-child(2) img').attr('style','display:none');
            $('#bm').attr('style','display:block');
           
            var animation = bodymovin.loadAnimation({
                container: document.getElementById('bm'),       //找到该元素容器添加
                renderer: 'svg',                                //'svg/canvas/html'
                loop: 1,                                        //播放次数 true / false / number
                autoplay: true,                                 //是否自动播放 true / false
                path: '/js/scene/channel/discover.json'         //对应的json路径
                name: "Hello World", // Name for future reference. Optional.
            });
            
            //js语法绑定一次播放完成事件
            animation.addEventListener('complete',function(){
                $('#bm').attr('style','display:none');
                $('.al-footerBarItem:nth-child(2) img').attr('style','display:block');
            }); 
            TempCache.setItem('isFirst','0');
        }else{
            $('.al-footerBarItem:nth-child(2) img').attr('style','display:block');
        }
    }
```

>   注：还可以使用对象的方式把初始化数据存在animData里面

    var animData = {
        ...
    };
    var anim = bodymovin.loadAnimation(animData);
                        
    事件
        1. complete 一次播放完成
        2. loopComplete 循环播放一次完成,每次都会触发
        3. enterFrame 播放过程中不断触发,慎用,在无性能瓶颈的情况下,最高触发次数为250fps,所以不要给这个事件.
        4. segmentStart 不同片段播放开始时候触发,如果是相同片段的循环,第一次后就不会触发此事件了.
        
-  [x] [jQuery-demo](http://airbnb.io/lottie/)  

### vue 使用 bodymovin 方法

>   1.需要安装bodymovin模块

    npm install bodymovin
 
>   2.在使用文件引入该模块

    import TempCache from "static/js/tempcache.js";     //该文件是localStorage封装的方法
    import bodymovin from 'bodymovin';
    import * as animationData from "static/js/assets/discover.json";
    
>   3.在template中使用

     <img v-show="!animates" src="data:image/png;base64..."/>
     <div v-show="animates" ref="lavContainer"></div>

>   4.初始化的时候使用该动画

```
    //首次进入的时候[发现图标]执行动画，以后在进的时候均无动画，除非清除缓存
    props:['isActive'],
    data(){
        return {
            animates = true,        //控制动画是否显示，该属性用动画显示完成之后替换原来base64的图
            defaultOptions:{animationData:animationData}
        }
    },
    mounted(){
        let t = this;
        if(TempCache.getItem('isFirstAnimation') && TempCache.getItem('isFirstAnimation')==0){
            t.animates = false;
        }else{
            if(t.isActive==0){      //isActive是各页面传递过来的参数，0首页，1发现，2消息，3我的
                var animation= bodymovin.loadAnimation({
                        container: t.$refs.lavContainer,
                        renderer: 'svg',
                        loop: 1,
                        autoplay: true,
                        animationData: t.defaultOptions.animationData,
                    }
                );
                animation.addEventListener('complete',function(){
                    t.animates = false;
                });
                TempCache.setItem('isFirstAnimation','0');
            }else{
                t.animates = false;
            }
        }
    },
```
>   使用详情参见

-  [x] [Vue-demo](http://airbnb.io/lottie/)   
   

    



































