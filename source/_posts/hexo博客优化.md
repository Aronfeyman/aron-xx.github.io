---
layout: layout
date: 2021-04-23 16:42:20
title:
tags:
---

# hexo博客优化

## 图片懒加载
> 安装**hexo-lazyload-image**插件

*  在本地bloger根目录下打开powershell，执行**npm install hexo-lazyload-image --save**
*  在 \_config.yml下添加以下代码
```yml 
lazyload:
  enable: true  # 是否开启图片懒加载
  onlypost: false  # 是否只对文章的图片做懒加载
  loadingImg: # eg ./images/loading.gif
```
*  最后执行 **hexo clean** &&**hexo g -d ** && **hexo s **命令

## 代码压缩
> 安装 **gulp**插件
> 参考 [Hexo使用Gulp压缩静态资源](https://www.voidking.com/dev-hexo-gulp/)

* 在本地bloger根目录下打开powershell，执行**npm install gulp -g** [^footnote]
[^footnote]: 此处添加**-g**，表示全局安装的意思.
> npm install gulp --save
> npm install gulp-htmlclean gulp-htmlmin gulp-clean-css gulp-uglify gulp-imagemin --save
> npm install gulp-babel babel-preset-env babel-preset-mobx --save
> npm install -D @babel/core @babel/preset-react @babel/preset-env --save

* 在 bloger根目录新建文件 gulpfile.js，把以下代码下载到此文件中
```js
var gulp = require("gulp");
var debug = require("gulp-debug");
var cleancss = require("gulp-clean-css"); //css压缩组件
var uglify = require("gulp-uglify"); //js压缩组件
var htmlmin = require("gulp-htmlmin"); //html压缩组件
var htmlclean = require("gulp-htmlclean"); //html清理组件
var imagemin = require("gulp-imagemin"); //图片压缩组件
var changed = require("gulp-changed"); //文件更改校验组件
var gulpif = require("gulp-if"); //任务 帮助调用组件
var plumber = require("gulp-plumber"); //容错组件（发生错误不跳出任务，并报出错误内容）
var isScriptAll = true; //是否处理所有文件，(true|处理所有文件)(false|只处理有更改的文件)
var isDebug = true; //是否调试显示 编译通过的文件
var gulpBabel = require("gulp-babel");
var es2015Preset = require("babel-preset-es2015");
var del = require("del");
var Hexo = require("hexo");
var hexo = new Hexo(process.cwd(), {}); // 初始化一个hexo对象

// 清除public文件夹
gulp.task("clean", function () {
    return del(["public/**/*"]);
});

// 下面几个跟hexo有关的操作，主要通过hexo.call()去执行，注意return
// 创建静态页面 （等同 hexo generate）
gulp.task("generate", function () {
    return hexo.init().then(function () {
        return hexo
            .call("generate", {
                watch: false
            })
            .then(function () {
                return hexo.exit();
            })
            .catch(function (err) {
                return hexo.exit(err);
            });
    });
});

// 启动Hexo服务器
gulp.task("server", function () {
    return hexo
        .init()
        .then(function () {
            return hexo.call("server", {});
        })
        .catch(function (err) {
            console.log(err);
        });
});

// 部署到服务器
gulp.task("deploy", function () {
    return hexo.init().then(function () {
        return hexo
            .call("deploy", {
                watch: false
            })
            .then(function () {
                return hexo.exit();
            })
            .catch(function (err) {
                return hexo.exit(err);
            });
    });
});

// 压缩public目录下的js文件
gulp.task("compressJs", function () {
    return gulp
        .src(["./public/**/*.js", "!./public/libs/**"]) //排除的js
        .pipe(gulpif(!isScriptAll, changed("./public")))
        .pipe(gulpif(isDebug, debug({ title: "Compress JS:" })))
        .pipe(plumber())
        .pipe(
            gulpBabel({
                presets: [es2015Preset] // es5检查机制
            })
        )
        .pipe(uglify()) //调用压缩组件方法uglify(),对合并的文件进行压缩
        .pipe(gulp.dest("./public")); //输出到目标目录
});

// 压缩public目录下的css文件
gulp.task("compressCss", function () {
    var option = {
        rebase: false,
        //advanced: true, //类型：Boolean 默认：true [是否开启高级优化（合并选择器等）]
        compatibility: "ie7" //保留ie7及以下兼容写法 类型：String 默认：''or'*' [启用兼容模式； 'ie7'：IE7兼容模式，'ie8'：IE8兼容模式，'*'：IE9+兼容模式]
        //keepBreaks: true, //类型：Boolean 默认：false [是否保留换行]
        //keepSpecialComments: '*' //保留所有特殊前缀 当你用autoprefixer生成的浏览器前缀，如果不加这个参数，有可能将会删除你的部分前缀
    };
    return gulp
        .src(["./public/**/*.css", "!./public/**/*.min.css"]) //排除的css
        .pipe(gulpif(!isScriptAll, changed("./public")))
        .pipe(gulpif(isDebug, debug({ title: "Compress CSS:" })))
        .pipe(plumber())
        .pipe(cleancss(option))
        .pipe(gulp.dest("./public"));
});

// 压缩public目录下的html文件
gulp.task("compressHtml", function () {
    var cleanOptions = {
        protect: /<\!--%fooTemplate\b.*?%-->/g, //忽略处理
        unprotect: /<script [^>]*\btype="text\/x-handlebars-template"[\s\S]+?<\/script>/gi //特殊处理
    };
    var minOption = {
        collapseWhitespace: true, //压缩HTML
        collapseBooleanAttributes: true, //省略布尔属性的值 <input checked="true"/> ==> <input />
        removeEmptyAttributes: true, //删除所有空格作属性值 <input id="" /> ==> <input />
        removeScriptTypeAttributes: true, //删除<script>的type="text/javascript"
        removeStyleLinkTypeAttributes: true, //删除<style>和<link>的type="text/css"
        removeComments: true, //清除HTML注释
        minifyJS: true, //压缩页面JS
        minifyCSS: true, //压缩页面CSS
        minifyURLs: true //替换页面URL
    };
    return gulp
        .src("./public/**/*.html")
        .pipe(gulpif(isDebug, debug({ title: "Compress HTML:" })))
        .pipe(plumber())
        .pipe(htmlclean(cleanOptions))
        .pipe(htmlmin(minOption))
        .pipe(gulp.dest("./public"));
});

// 压缩 public/medias 目录内图片
gulp.task("compressImage", function () {
    var option = {
        optimizationLevel: 5, //类型：Number 默认：3 取值范围：0-7（优化等级）
        progressive: true, //类型：Boolean 默认：false 无损压缩jpg图片
        interlaced: false, //类型：Boolean 默认：false 隔行扫描gif进行渲染
        multipass: false //类型：Boolean 默认：false 多次优化svg直到完全优化
    };
    return gulp
        .src("./public/medias/**/*.*")
        .pipe(gulpif(!isScriptAll, changed("./public/medias")))
        .pipe(gulpif(isDebug, debug({ title: "Compress Images:" })))
        .pipe(plumber())
        .pipe(imagemin(option))
        .pipe(gulp.dest("./public"));
});
// 执行顺序： 清除public目录 -> 产生原始博客内容 -> 执行压缩混淆 -> 部署到服务器
gulp.task(
    "build",
    gulp.series(
        "clean",
        "generate",
        "compressHtml",
        "compressCss",
        "compressJs",
        "compressImage",
        gulp.parallel("deploy")
    )
);

// 默认任务
gulp.task(
    "default",
    gulp.series(
        "clean",
        "generate",
        gulp.parallel("compressHtml", "compressCss", "compressJs","compressImage")
    )
);
//Gulp4最大的一个改变就是gulp.task函数现在只支持两个参数，分别是任务名和运行任务的函数

```

* 修改travis配置
> 在bloger根目录下的.travis.yml文件中添加以下代码
```yml
sudo: false
language: node_js
node_js:
- 10.16.3
cache: npm
branches:
  only:
  - master # build master branch only

env:
  global:
  - GIT_USER: voidking ###修改成自己的github账户名,以下voidking都要改.
  - HEXO_BACKUP_REPO: github.com/voidking/hexo-backup.git
  - HEXO_THEME_REPO: github.com/voidking/hexo-theme-next.git
  - GITHUB_PAGES_REPO: github.com/voidking/voidking.github.io.git
  - VOIDKING_REPO: github.com/voidking/voidking.git

before_install:
- export TZ='Asia/Shanghai'
- npm install hexo -g
- npm install gulp-cli -g

install:
- npm install

script:
- git clone https://${HEXO_THEME_REPO} themes/next
- git clone https://${GIT_USER}:${GITHUB_TOKEN}@${HEXO_BACKUP_REPO} hexo-backup
- mv hexo-backup/source .
- rm -rf source/private
- npm run build

after_success:
- git config --global user.name "voidking"
- git config --global user.email "voidking@qq.com"
- git clone https://${GIT_USER}:${GITHUB_TOKEN}@${GITHUB_PAGES_REPO} voidking
- unalias cp
- cp -rf public/. voidking
- cd voidking
- git add .
- git commit -m "Travis CI Auto Builder"
- git push --force --quiet "https://${GIT_USER}:${GITHUB_TOKEN}@${GITHUB_PAGES_REPO}" master:master
```

* 使用了gulp时候，构建发布需要四个命令
> hexo clean
> hexo g
> gulp
> hexo d

* 简化输入，直接构建两条命令 npm run bulid  &&  npm run deploy直接完成部署。
> 在bloger根目录下的package.json文件中添加以下代码
​```json
"scripts": {
  "build": "hexo clean && hexo g && gulp",
  "deploy": "hexo clean && hexo g && gulp && hexo d"
}


## CDN加速--Content Delivery Network
> CDN是构建在网络之上的内容分发网络，依靠部署在各地的边缘服务器，通过中心平台的负载均衡、内容分发、调度等功能模块，使用户就近获取所需内容，降低网络拥塞，提高用户访问响应速度和命中率。CDN的关键技术主要有内容存储和分发技术。

### jsDelivr + Github

## Travis CI使用
> Travis CI是在软件开发领域中的一个在线的，分布式的持续集成服务，用来构建及测试在GitHub托管的代码 

