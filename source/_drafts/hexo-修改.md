# hexo修改指南

>参考链接[hexo部署]("https://my.oschina.net/mflyyou/blog/3189579")
>参考链接[hexo详细部署]("https://blog.csdn.net/sinat_37781304/article/details/82729029")

## CDN加速
* 把 themes\hexo-theme-matery-master\source下的文件放到oss上去，利用CDN加速



## hexo命令

### hex draft 命令

####	==hexo new draft newpage==
*	这样会在source/_draft中新建一个newpage.md文件
####	==hexo server --draft==
*	在本地端口中开启服务预览
#### 	==hexo publish draft newpage==
*	就会自动把newpage.md发送到post中

