---
title: 利用hexo框架搭建博客
top: true
cover: true
categories: 工具
keywords: hexo搭建指南
tags:
  - hexo
summary: hexo搭建笔记
abbrlink: 22223
---







# 使用hexo搭建自己的blog

> 使用hexo框架配合hexo搭建自己的blog
> 预备知识： git操作命令和简单的批处理命令



## hexo简介
> 参考hexo官方文档 [hexo](https://hexo.io/zh-cn/docs/ )
> Hexo是高效的静态站点生成框架，它基于Node.js

### hexo常用命令


* hexo g -d 	直接完成页面生成渲染及上传
* hexo s |hexo server   启动服务器



### hexo主题部署 --基于hexo-matery主题

> 修改一些参数，展现出符合自己审美的状态。


#### 去掉banner颜色动画

* 在 ./theme文件夹下找到matery.css文件，使用快捷键(*ctrl+F*) 找出代码中的**.bg-cover:after**部分。注视掉即可

```css
	.bg-cover:after {
    -webkit-animation: rainbow 60s infinite;
    animation: rainbow 60s infinite;
} 

```
#### 安装live2d动画模型

> 参考教程：[hexo-helper-live2d]("https://github.com/EYHN/hexo-helper-live2d/blob/master/README.zh-CN.md")

* 在博客根目录下打开powershell，使用命令  npm install --save hexo-helper-live2d



#### 创建自己的专属域名

> 通过阿里云这个域名代理商，把**yourname.github.io**换成**yourname.com**s

#### CDN访问加速
> 简介


#### 点击鼠标显示烟花效果
> 转载[LRBlog]("https://liuruibin.com/posts/64eb.html")

* 首先在themes/matery/source/js目录下新建**fireworks.js**文件，将链接中的内容复制到fireworks.js文件内。[fireworks文件](https://cdn.jsdelivr.net/gh/Yafine/cdn@3.2.5/source/js/fireworks.js)

* 然后再themes/matery/layout/layout.ejs文件内添加下面的内容

```ejs
<canvas class="fireworks" style="position: fixed;left: 0;top: 0;z-index: 1; pointer-events: none;" ></canvas> 
<script type="text/javascript" src="//cdn.bootcss.com/animejs/2.2.0/anime.min.js"></script> 
<script type="text/javascript" src="/js/fireworks.js"></script>
```
***

> 添加樱花飘落和雪花飘落效果 在themes/matery/source/js目录下新建**sakura.js** **snow.js**   [sakura.js文件]("https://cdn.jsdelivr.net/gh/Yafine/cdn@3.2.5/source/js/sakura.js")  
[snow.js文件]("https://cdn.jsdelivr.net/gh/Yafine/cdn@3.2.5/source/js/snow1.js")

*  然后再themes/matery/layout/layout.ejs文件内添加下面的内容
```ejs 
<script type="text/javascript">
//只在桌面版网页启用特效
var windowWidth = $(window).width();
if (windowWidth > 768) {
    document.write('<script type="text/javascript" src="/js/sakura.js"><\/script>');
}
</script>

<script src="/js/snow.js"></script>
```


### 网站提速

> 网页打开缓慢 ，可行的操作

* 	尝试安装 hexo-all-minifier 插件来压缩文件
*	减少不必要的 js 插件，例如字数统计、动态背景。
#### 查找并解决拖慢速度的资源，以 Chrome 浏览器为例：
* 页面中点击右键，选择「检查」
* 在右边的窗口中「Network」选项卡，并勾选「Disable cache」
####  刷新网页，查看加载速度慢的资源:
* 加载缓慢的图片，建议使用 CDN
* 加载缓慢的可以不用的 js 插件，建议舍弃
* 加载缓慢却必须使用的 js 插件，建议下载并自己上传至 **jsdelivr**