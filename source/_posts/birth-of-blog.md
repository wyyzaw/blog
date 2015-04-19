title: "博客诞生记"
date: 2015-04-19 15:30:59
tags:
- "寂寞的周末"
- "hexo"
---

## 背景
又是一个单身狗的周末，本来打算是看看公演堕落堕落两天就过去了。结果临到出门之前又懒癌发作不想动（=_=!）
就试着在群里原价叫卖门票，正巧遇上个迫不及待想去见见四期生的聚聚，结果就是在开演前的8小时把辛辛苦苦守了两天抢到的门票原价转卖了（我碎成渣渣的黄牛心啊....）
既然不去堕落了，那想着就找点事做吧~一直以来都觉得自己好歹是个未来的程序猿，连个技术博客都没有也太寒酸了，正巧周末无聊，就弄个玩玩吧。  
<!-- more -->
说干就干  
之前在学习过程中也通过度娘看到过很多形形色色的技术博客，比如[csdn](http://blog.csdn.net/)啊，[oschina](http://www.oschina.net/blog)啊，[iteye](http://www.iteye.com/blogs)等等，每个看起来都很好用，也有很多大牛在上面分享自己的技术。但最后想了想还是自己搭一个吧，因为

> 1. 这些技术博客都是很专业的“技术博客”，而我想要的其实是个记录自己的空间，可能是技术，可能是一些想法，可能只是纯粹的牢骚。而类似CSND，简书之类的大一点的网络平台，其实上面的文章大多都有自己特定的一套风格。这并非坏处，但或许不太适合我（无法想象在CSDN上发一篇美少女偶像的安利文是什么画面）
> 2. 作为学习网页开发的程序猿，至今没有自己从域名到dns到空间亲力亲为的建过站，实在是耻辱啊！

基于以上两点，最终决定自己来搭个博客，同时也将自己动手的过程记录下来，算是作为这个博客的第一篇技术文吧，よっしゃいくぞー

---

## Part I.在摇篮中出生
尤记得之前在视奸某个微博大牛的时候，发现他的博客是用一个叫hexo的程序搭的，感觉还不错，就决定是你了！

### 一.什么是hexo?
hexo是一个由台湾大学生tommy351写的，基于Node.Js的静态博客框架；现在同类程序这么多，他为什么还要写这么个东西呢？原因可参见这篇博文*[Hexo 颯爽登場！](http://zespia.tw/blog/2012/10/11/hexo-debut/)*(一看开头图就知道这哥们乃同道中人~)

### 二.安装Hexo

> 由于我电脑的系统是CentOS，以下过程均为CentOS系统中的操作过程


#### 1.安装Node.JS
hexo基于node，所以第一步先安装node，方法很简单，上[官网](https://nodejs.org/download/)下载对应自己系统的最新版node程序，然后执行以下命令解压即可  

	tar -xvzf node-v0.12.2-linux-x64.tar.gz  

可以把解压后的bin目录添加到系统PATH里，以后执行起来更方便

#### 2.安装hexo
hexo的安装十分方便，只需一条指令

	npm install -g hexo-cli

#### 3.安装博客
在执行完上一条指令之后，hexo已经安装到系统里，但还需要初始化后才能使用，指令如下

	$ hexo init <folder>
	$ cd <folder>
	$ npm install

初始化后，hexo即可正常使用

### 三.使用Hexo
#### 1.启动博客服务器
经过上面的初始化过程，你的博客文件夹里会生成如下的目录结构

	.
	├── _config.yml
	├── package.json
	├── scaffolds
	├── scripts
	├── source
	|   ├── _drafts
	|   └── _posts
	└── themes

此时在博客文件夹中执行

	hexo server

即可在本机4000端口启动博客服务
通过浏览器访问`127.0.0.1:4000`即可看到默认的博客页面，其中会自动生成第一篇名为hello world的博文，上面有一些hexo的基本指令
![hexo初始页面](/css/images/birth-of-blog/1.png)

#### 2.第一篇博文
有了服务器，怎么添加博文呢，也非常简单，在你的博客目录下执行

	hexo new "博文名"

即可在`blogpath/source/_posts`下生成名为`博文名.md`的文件(也可自行在目录下建立md文件)，将你想要写的内容输入进去，服务器就会自动将此文章渲染为对应的博文；博文可使用*markdown*格式书写

> *科普：Markdown是一种可以使用普通文本编辑器编写的标记语言，通过类似HTML的标记语法，它可以使普通文本内容具有一定的格式 [markdown语法说明](http://www.appinn.com/markdown/)*

---

## Part II.全世界都能看到我
上面介绍的只是在本机上运行hexo的方法，但如果想要让所有人都能看到你写的东西，还需要把他发布到网上，对此hexo也有很方便的发布方式


### 一.Github Page
[Github Pages](https://pages.github.com/)是Github提供的一个免费静态站点
>Github及git的使用方式参考[github帮助页面](https://help.github.com/)

#### 1.建立repo
首先在Github上建立一个空的Repo，我的repo地址为[https://github.com/wyyzaw/blog](https://github.com/wyyzaw/blog)

#### 2.将博客发布到Github
犹豫github page只支持静态页面，所以我们需要先通过指令将hexo中的博客页面及文章生成静态的html文件

	hexo generate

然后需要修改博客目录下的_config.yml文件，找到其中的deploy部分，设置刚才建立的repo地址

	deploy:
	  type: git
	  repo: git@github.com:wyyzaw/blog.git

然后就可以通过deploy命令将项目部署至Github

	hexo deploy

部署后页面如下

![Github repo页面](/css/images/birth-of-blog/2.png)

检查一下分支是否为*gh-pages*，这是Githun特别为web项目设置的分支
点击项目页面右侧的settings-github pages，可看到提示：Your site is published at http://wyyzaw.github.io/blog
访问此页面即可看到你的博客，但此时由于尚未配置路径，所以页面上的css文件和图片显示错误

![博客页面](/css/images/birth-of-blog/3.png)

#### 3.自定义域名
通过[http://www.dot.tk/en/index.html?lang=en](http://www.dot.tk/en/index.html?lang=en)网站可以申请免费的tk域名，申请完成后配置dns  
添加两条指向`192.30.252.153/154`的A记录

![dns配置](/css/images/birth-of-blog/4.png)

在Github项目目录下建立CNAME文件，内容为刚申请的域名  
刚设置的dns需要等待最多15分钟来让他同步到dns记录中，然后即可通过刚申请的域名正常访问网站

>备注1：具体CNAME配置可参考[帮助页面](https://help.github.com/articles/setting-up-a-custom-domain-with-github-pages/)  
>备注2：hexo的发布功能仅会把生成的静态页面文件发布到Github，如想在不同电脑上编辑博客可将博客目录同步到Github的master分支

---

## Part III.我在成长
自己的博客页面相比其他网站最大的好处是什么？当然是可以自定义页面！

### 一.增加评论功能
因为Github page只提供静态页面功能，所以想要为自己的博客增加评论功能就要通过第三方提供的服务来实现，在此方面国外比较著名的有[disqus](https://disqus.com/)，国内的有[多说](http://duoshuo.com/)，[畅言](http://changyan.kuaizhan.com/)，[友言](http://www.uyan.cc/)等等，我选择的是友言，其实设置起来都是一样的  
* 首先在第三方的网页上面获取嵌入用的代码  
![评论潜入代码](/css/images/birth-of-blog/5.png)
* 然后将代码粘贴到hexo的文章页模版中你想要评论出现的位置，我放在文章底部的tag上面(路径为`blogpath/themes/landscape/layout/_partial/artical.ejs`)

```
		<% if(!index){ %>
			<!-- comment part -->
			<!-- UY BEGIN -->
			<div id="uyan_frame"></div>
			<script type="text/javascript" src="http://v2.uyan.cc/code/uyan.js?uid=2024309"></script>
			<!-- UY END -->
		<% } %>
		<%- partial('post/tag') %>
```

>hexo使用的模版引擎是NodeJS的ejs模板，相信有一点web知识的人一看就知道怎么用了，十分方便

配置完成后即可在文章页面中看到评论框了
![评论](/css/images/birth-of-blog/6.png)

当然，除了评论之外一个完善的博客可能还有很多需要的功能，像Google分析，百度统计，微薄转发等，也可以自己开发一些插件！这些就留待以后研究了~
