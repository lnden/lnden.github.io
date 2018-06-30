---
title: Hexo搭建博客
date: 2018-01-24 11:12:28
tags:
---

### 使用Hexo框架创建一个Github博客
&emsp;&emsp;    [Hexo官网](https://hexo.io/)    

&emsp;&emsp;    [Github官网](https://github.com/)
    
下面是一个创建blog的简单教程，在这里我默认你已经使用过github仓库，并且已经安装Git和Node


>### 安装Git、Node.js

&emsp;&emsp;    [Git官网](https://git-scm.com/)&emsp;&emsp;   进入官网点击Downloads下载，下载到本地并且安装(这里可以一键安装也可以安装到指定目录下)
    
&emsp;&emsp;    [Node官网](https://nodejs.org/zh-cn/)&emsp;&emsp;     进入官网选着[推荐]下载，下载到本地并且安装(这里可以一键安装也可以安装到指定目录下)


>### 创建仓库

&emsp;&emsp;    1.进入github创建一个新的仓库，这里一定要注意仓库的名称
&emsp;&emsp;&emsp;&emsp;    username.github.io  //其中username为你的账号名称
&emsp;&emsp;    2.创建分支自定义分支[hexo]
&emsp;&emsp;&emsp;&emsp;    这里解释一下为什么要创建一个分支呢，因为我们需要把博客搭建的基础文件存放起来，
&emsp;&emsp;&emsp;&emsp;    以后添加文章或者更改配置的时候便于维护
&emsp;&emsp;&emsp;&emsp;    这样我们基础文件存放在hexo分支下，发布展示的内容存放在master分支
&emsp;&emsp;    3.设置hexo为默认分支
&emsp;&emsp;&emsp;&emsp;    1)、进入刚才新创建的仓库，点击仓库名称下，导航最后一个settings按钮
&emsp;&emsp;&emsp;&emsp;    2)、进入设置页面，点击左侧导航branches按钮
&emsp;&emsp;&emsp;&emsp;    3)、更改Default branch里面的默认分支为hexo
&emsp;&emsp;    4.将仓库复制到本地 git clone https://github.com/username/username.github.io.git 此处为你的仓库连接
注：如果你已经完成以上步骤，cloen成功之后进入cd username.github.io文件内，执行git branch 就会看见当前分支
    
>### 安装hexo

&emsp;&emsp;    1.首先全局安装hexo
&emsp;&emsp;&emsp;&emsp;    cnpm install -g hexo
&emsp;&emsp;    2.初始化一个hexo项目
&emsp;&emsp;&emsp;&emsp;    1).可以新建文件夹，进入新建文件夹内 hexo init
&emsp;&emsp;&emsp;&emsp;    2).可以直接使用 hexo init 自定义名称[hexo]
&emsp;&emsp;    3.需要安装部署模块，把项目部署到master分支，展示blog效果
&emsp;&emsp;&emsp;&emsp;    1).如果你是创建文件夹，进入文件夹 hexo init 那就直接执行下面语句进行安装
&emsp;&emsp;&emsp;&emsp;    2).如果你是hexo init 自定义名称[hexo] 那就cd 文件夹内进行安装
&emsp;&emsp;&emsp;&emsp;    cnpm install hexo-deployer-git --save
&emsp;&emsp;    5.把自定义名称[hexo]内文件复制到刚才下载的仓库里面
&emsp;&emsp;    需要拷贝的文件如下：
&emsp;&emsp;&emsp;&emsp;    .gitignore          &emsp;&emsp;&emsp;&emsp;    //git忽略的文件
&emsp;&emsp;&emsp;&emsp;    _config.yml         &emsp;&emsp;&emsp;&emsp;    //hexo主配置文件
&emsp;&emsp;&emsp;&emsp;    package.json        &emsp;&emsp;&emsp;&emsp;    //hexo需要的依赖模块
&emsp;&emsp;&emsp;&emsp;    source/             &emsp;&emsp;&emsp;&emsp;    //静态资源.md文件
&emsp;&emsp;&emsp;&emsp;    themes/             &emsp;&emsp;&emsp;&emsp;    //主题文件目录
&emsp;&emsp;&emsp;&emsp;    scaffolds/          &emsp;&emsp;&emsp;&emsp;    //存放模块
注：这里面有一个小问题就是，正常情况下可以在clone下的仓库内 hexo init 初始化

>### 部署静态文件

&emsp;&emsp;    1.更改配置文件
&emsp;&emsp;    deploy:
&emsp;&emsp;&emsp;&emsp;    type: git
&emsp;&emsp;&emsp;&emsp;    repo: git@github.com:username/username.github.io.git
&emsp;&emsp;&emsp;&emsp;    branch: master
&emsp;&emsp;    2.hexo clean        &emsp;&emsp;    //清除缓存
&emsp;&emsp;    3.hexo generate     &emsp;&emsp;    //生成静态文件
&emsp;&emsp;    4.hexo deploy       &emsp;&emsp;    //把生成的静态文件部署到github仓库上为master
&emsp;&emsp;    5.可以在网址输入username.github.io看看
    
>### 把基础文件提交hexo分支

&emsp;&emsp;    1.git add -A            &emsp;&emsp;    //-A全部提交
&emsp;&emsp;    2.git commit -m ""      
&emsp;&emsp;    3.git push origin hexo 


>### 使用next主题

&emsp;&emsp;    [Next主题](http://theme-next.iissnan.com/)
 
&emsp;&emsp;    1.git clone https://github.com/iissnan/hexo-theme-next themes/next