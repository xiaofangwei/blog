---
title: Next | Hexo主题的多种样式配置
date: 2018-03-17 11:58:50
top: 1
tags: hexo-next 
---
# 添加404界面

在source文件夹新建404.html，内容如下：
```html
	---
	layout: false
	title: "404"
	---
	<!DOCTYPE HTML>
	<html>
	<head>
	    <title>404 - No-Sky'blog</title>
	    <meta name="description" content="404错误，页面不存在！">
	    <meta http-equiv="content-type" content="text/html;charset=utf-8;"/>
	    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
	    <meta name="robots" content="all" />
	    <meta name="robots" content="index,follow"/>
	</head>
	<body>
	    <script type="text/javascript" src="http://qzonestyle.gtimg.cn/qzone_v6/lostchild/search_children.js" charset="utf-8"></script>
	</body>
	</html>
```
# 添加本地搜索功能

首先安装 hexo-generator-searchdb，在站点的根目录下执行以下命令：
```Bash
	$ npm install hexo-generator-searchdb --save
```
编辑 站点配置文件，新增以下内容到任意位置：
```yml
	search:
	  path: search.xml
	  field: post
	  format: html
	  limit: 10000
```
编辑 主题配置文件，启用本地搜索功能：
（在next主题的最新版已经有了这个配置，所以只要找local_search，把false改成true）
```yml
	# Local search
	local_search:
	  enable: true
```
# 修改标签样式

修改文章底部的那个带#号的标签
实现效果图

![](http://upload-images.jianshu.io/upload_images/5308475-9f1817d2d7627f7a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

具体实现方法
修改模板`/themes/next/layout/_macro/post.swig`，搜索 `rel="tag">#`，将` # `换成`<i class="fa fa-tag"></i>`

# 网站底部字数统计
实现效果图

![](http://upload-images.jianshu.io/upload_images/5308475-f26f21e2f2b34e18.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

具体方法实现
切换到根目录下，然后运行如下代码

	$ npm install hexo-wordcount --save

然后在/themes/next/layout/_partials/footer.swig文件尾部加上：
```html
	<div class="theme-info">
	  <div class="powered-by"></div>
	  <span class="post-count">博客全站共{{ totalcount(site) }}字</span>
	</div>
```
#  添加 README.md 文件
每个项目下一般都有一个 README.md 文件，但是使用 hexo 部署到仓库后，项目下是没有 README.md 文件的。
在 Hexo 目录下的 source 根目录下添加一个 README.md 文件，修改站点配置文件 _config.yml，将 skip_render 参数的值设置为
```
	skip_render: README.md
```
保存退出即可。再次使用 hexo d 命令部署博客的时候就不会在渲染 README.md 这个文件了
# 点击爆炸效果
实现效果图
![](http://upload-images.jianshu.io/upload_images/5308475-39a777c8c36cec1a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
实现方法:
跟那个红心是差不多的，首先在themes/next/source/js/src里面建一个叫fireworks.js的文件，代码如下：
```javascript
	"use strict";function updateCoords(e){pointerX=(e.clientX||e.touches[0].clientX)-canvasEl.getBoundingClientRect().left,pointerY=e.clientY||e.touches[0].clientY-canvasEl.getBoundingClientRect().top}function setParticuleDirection(e){var t=anime.random(0,360)*Math.PI/180,a=anime.random(50,180),n=[-1,1][anime.random(0,1)]*a;return{x:e.x+n*Math.cos(t),y:e.y+n*Math.sin(t)}}function createParticule(e,t){var a={};return a.x=e,a.y=t,a.color=colors[anime.random(0,colors.length-1)],a.radius=anime.random(16,32),a.endPos=setParticuleDirection(a),a.draw=function(){ctx.beginPath(),ctx.arc(a.x,a.y,a.radius,0,2*Math.PI,!0),ctx.fillStyle=a.color,ctx.fill()},a}function createCircle(e,t){var a={};return a.x=e,a.y=t,a.color="#F00",a.radius=0.1,a.alpha=0.5,a.lineWidth=6,a.draw=function(){ctx.globalAlpha=a.alpha,ctx.beginPath(),ctx.arc(a.x,a.y,a.radius,0,2*Math.PI,!0),ctx.lineWidth=a.lineWidth,ctx.strokeStyle=a.color,ctx.stroke(),ctx.globalAlpha=1},a}function renderParticule(e){for(var t=0;t<e.animatables.length;t++){e.animatables[t].target.draw()}}function animateParticules(e,t){for(var a=createCircle(e,t),n=[],i=0;i<numberOfParticules;i++){n.push(createParticule(e,t))}anime.timeline().add({targets:n,x:function(e){return e.endPos.x},y:function(e){return e.endPos.y},radius:0.1,duration:anime.random(1200,1800),easing:"easeOutExpo",update:renderParticule}).add({targets:a,radius:anime.random(80,160),lineWidth:0,alpha:{value:0,easing:"linear",duration:anime.random(600,800)},duration:anime.random(1200,1800),easing:"easeOutExpo",update:renderParticule,offset:0})}function debounce(e,t){var a;return function(){var n=this,i=arguments;clearTimeout(a),a=setTimeout(function(){e.apply(n,i)},t)}}var canvasEl=document.querySelector(".fireworks");if(canvasEl){var ctx=canvasEl.getContext("2d"),numberOfParticules=30,pointerX=0,pointerY=0,tap="mousedown",colors=["#FF1461","#18FF92","#5A87FF","#FBF38C"],setCanvasSize=debounce(function(){canvasEl.width=2*window.innerWidth,canvasEl.height=2*window.innerHeight,canvasEl.style.width=window.innerWidth+"px",canvasEl.style.height=window.innerHeight+"px",canvasEl.getContext("2d").scale(2,2)},500),render=anime({duration:1/0,update:function(){ctx.clearRect(0,0,canvasEl.width,canvasEl.height)}});document.addEventListener(tap,function(e){"sidebar"!==e.target.id&&"toggle-sidebar"!==e.target.id&&"A"!==e.target.nodeName&&"IMG"!==e.target.nodeName&&(render.play(),updateCoords(e),animateParticules(pointerX,pointerY))},!1),setCanvasSize(),window.addEventListener("resize",setCanvasSize,!1)}"use strict";function updateCoords(e){pointerX=(e.clientX||e.touches[0].clientX)-canvasEl.getBoundingClientRect().left,pointerY=e.clientY||e.touches[0].clientY-canvasEl.getBoundingClientRect().top}function setParticuleDirection(e){var t=anime.random(0,360)*Math.PI/180,a=anime.random(50,180),n=[-1,1][anime.random(0,1)]*a;return{x:e.x+n*Math.cos(t),y:e.y+n*Math.sin(t)}}function createParticule(e,t){var a={};return a.x=e,a.y=t,a.color=colors[anime.random(0,colors.length-1)],a.radius=anime.random(16,32),a.endPos=setParticuleDirection(a),a.draw=function(){ctx.beginPath(),ctx.arc(a.x,a.y,a.radius,0,2*Math.PI,!0),ctx.fillStyle=a.color,ctx.fill()},a}function createCircle(e,t){var a={};return a.x=e,a.y=t,a.color="#F00",a.radius=0.1,a.alpha=0.5,a.lineWidth=6,a.draw=function(){ctx.globalAlpha=a.alpha,ctx.beginPath(),ctx.arc(a.x,a.y,a.radius,0,2*Math.PI,!0),ctx.lineWidth=a.lineWidth,ctx.strokeStyle=a.color,ctx.stroke(),ctx.globalAlpha=1},a}function renderParticule(e){for(var t=0;t<e.animatables.length;t++){e.animatables[t].target.draw()}}function animateParticules(e,t){for(var a=createCircle(e,t),n=[],i=0;i<numberOfParticules;i++){n.push(createParticule(e,t))}anime.timeline().add({targets:n,x:function(e){return e.endPos.x},y:function(e){return e.endPos.y},radius:0.1,duration:anime.random(1200,1800),easing:"easeOutExpo",update:renderParticule}).add({targets:a,radius:anime.random(80,160),lineWidth:0,alpha:{value:0,easing:"linear",duration:anime.random(600,800)},duration:anime.random(1200,1800),easing:"easeOutExpo",update:renderParticule,offset:0})}function debounce(e,t){var a;return function(){var n=this,i=arguments;clearTimeout(a),a=setTimeout(function(){e.apply(n,i)},t)}}var canvasEl=document.querySelector(".fireworks");if(canvasEl){var ctx=canvasEl.getContext("2d"),numberOfParticules=30,pointerX=0,pointerY=0,tap="mousedown",colors=["#FF1461","#18FF92","#5A87FF","#FBF38C"],setCanvasSize=debounce(function(){canvasEl.width=2*window.innerWidth,canvasEl.height=2*window.innerHeight,canvasEl.style.width=window.innerWidth+"px",canvasEl.style.height=window.innerHeight+"px",canvasEl.getContext("2d").scale(2,2)},500),render=anime({duration:1/0,update:function(){ctx.clearRect(0,0,canvasEl.width,canvasEl.height)}});document.addEventListener(tap,function(e){"sidebar"!==e.target.id&&"toggle-sidebar"!==e.target.id&&"A"!==e.target.nodeName&&"IMG"!==e.target.nodeName&&(render.play(),updateCoords(e),animateParticules(pointerX,pointerY))},!1),setCanvasSize(),window.addEventListener("resize",setCanvasSize,!1)};
```
打开themes/next/layout/_layout.swig,在</body>上面写下如下代码：
```javascript
	{% if theme.fireworks %}
	   <canvas class="fireworks" style="position: fixed;left: 0;top: 0;z-index: 1; pointer-events: none;" ></canvas> 
	   <script type="text/javascript" src="//cdn.bootcss.com/animejs/2.2.0/anime.min.js"></script> 
	   <script type="text/javascript" src="/js/src/fireworks.js"></script>
	{% endif %}
```
打开主题配置文件，在里面最后写下：
```yml
	# Fireworks
	   fireworks: true
```
更多的请参考
[Net主题配置](https://segmentfault.com/a/1190000009544924#articleHeader21)