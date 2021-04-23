

# 利用hexo搭建博客之基础教程

> 参考资料
>  [hexo搭建](https://www.cnblogs.com/mfrank/p/12829882.html#%E6%9C%AC%E5%9C%B0%E5%AE%89%E8%A3%85-nodejs "比较完善")
>  [hexo主题](https://easyhexo.com/2-Theme-use-and-config/2-5-hexo-theme-material/#material-%E4%B8%BB%E9%A2%98%E6%BC%94%E7%A4%BA)
>  [hexo官方文档](https://hexo.io/zh-cn/docs/index.html)
>  [hexo-theme-matery主题官方文档](https://github.com/blinkfox/hexo-theme-matery)
>  [hexo搭建教程](https://blog.csdn.net/sinat_37781304/article/details/82729029)
>  [Hexo配置Travis CI自动发布](https://www.voidking.com/dev-hexo-travis-ci/)
>  本文是介绍基于hexo框架，利用hexo-theme-matery搭建bloger的教程.

## 配置hexo环境
>预备知识
>git命令[廖雪峰git教程](https://www.liaoxuefeng.com/wiki/896043488029600)
>了解windows系统下的powershell用法
>了解windows系统下的包管理工具-chocolatey [chocolatey官方文档](https://docs.chocolatey.org/en-us/)
>了解npm包管理工具,知道简单的命令.






## travis ci使用
>有两种类型的 github pages，一种是使用 用户名.github.io 作为项目名，一种是使用其它名称。虽然看起来只是名字不一样，但两种方式其实是有差异的，**前一种方式里**，网页静态文件只能存放在 master 分支，所以如果想要把博客源文件也存到同一个仓库，必须使用其它分支来存放，**相应的 travis ci 监听和推送的分支也需要修改**，当然也可以使用另一个新的仓库来存放。**后一种方式**则没这个限制，通常使用名为 gh-pages 作为分支名，Hexo 内默认设置的分支也是叫这个名字。这里我们使用的是后一种方案，即源文件和生成的网页静态文件存放在同一个仓库，源文件在 master 分支，静态文件在 gh-pages 分支。

