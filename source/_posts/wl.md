---
title: 网站访问速度优化
tags: 网站
categories: 技术
cover: 'https://ae01.alicdn.com/kf/Hfd1bc67168a54360af07bed262920e56d.jpg'
abbrlink: 72eb1396
date: 2021-11-29 15:00:30
---

本文借鉴[呆逼の博客](https://blog.keepdai.cn/)丶[Akilarの糖果屋](https://akilar.top/)整理而成，用于记录与学习。

## CDN 加速

CDN怎么使用我这里就不讲了，这里记录一下相关的设置。



在访问控制这里找到ip访问限制和下行限速

[![img](https://cdn.jsdelivr.net/gh/L-20021213/picture@1.0.2/img/20210718000914.png)](https://cdn.jsdelivr.net/gh/L-20021213/picture@1.0.2/img/20210718000914.png)



[![img](https://cdn.jsdelivr.net/gh/L-20021213/picture@1.0.2/img/20210718000930.png)](https://cdn.jsdelivr.net/gh/L-20021213/picture@1.0.2/img/20210718000930.png)



在缓存配置里强烈推荐遵循源站,就是你原网站修改内容,他才会更新缓存。

[![img](https://cdn.jsdelivr.net/gh/L-20021213/picture@1.0.2/img/20210718000944.png)](https://cdn.jsdelivr.net/gh/L-20021213/picture@1.0.2/img/20210718000944.png)



带宽建议设置成5-10m。

[![img](https://cdn.jsdelivr.net/gh/L-20021213/picture@1.0.2/img/20210718001107.png)](https://cdn.jsdelivr.net/gh/L-20021213/picture@1.0.2/img/20210718001107.png)



## Gulp 压缩全站静态资源

[gulp](https://www.gulpjs.com.cn/) 能够帮助用户自动压缩静态资源，配合各类下属插件，能够压缩包括 css、js、html 乃至各类格式的图片文件。

- 安装 Gulp 插件：在博客根目录 `[Blogroot]` 打开终端，输入：

```
BASH
npm install --global gulp-cli
```

- 安装各个下属插件以实现对各类静态资源的压缩。

压缩 HTML：

```
BASH
npm install gulp-htmlclean --save-dev
npm install gulp-html-minifier-terser --save-dev
# 用gulp-html-minifier-terser可以压缩HTML中的ES6语法
```

压缩 CSS：

```
BASH
npm install gulp-clean-css --save-dev
```

压缩 JS：
[gulp-terser](https://github.com/duan602728596/gulp-terser) 只会直接压缩 js 代码，所以不存在因为语法变动导致的错误 。事实上，当我们使用 `jsdelivr` 的 `CDN` 服务时，只需要在 `css` 或者 `js` 的后缀前添加`.min`, 例如 `example.js->example.min.js`,`JsDelivr` 就会自动使用 `terser` 帮我们压缩好代码。

```
BASH
npm install gulp-terser --save-dev
```

- 为 Gulp 创建 `gulpfile.js` 任务脚本。在博客根目录 `[Blogroot]` 下新建 `gulpfile.js`, 打开 `[Blogroot]\gulpfile.js`, 输入以下内容：

```
JS

//用到的各个插件
var gulp = require('gulp');
var cleanCSS = require('gulp-clean-css');
var htmlmin = require('gulp-html-minifier-terser');
var htmlclean = require('gulp-htmlclean');
// gulp-tester
var terser = require('gulp-terser');
// 压缩js
gulp.task('compress', () =>
  gulp.src(['./public/**/*.js', '!./public/**/*.min.js'])
    .pipe(terser())
    .pipe(gulp.dest('./public'))
)
//压缩css
gulp.task('minify-css', () => {
    return gulp.src(['./public/**/*.css'])
        .pipe(cleanCSS({
            compatibility: 'ie11'
        }))
        .pipe(gulp.dest('./public'));
});
//压缩html
gulp.task('minify-html', () => {
    return gulp.src('./public/**/*.html')
        .pipe(htmlclean())
        .pipe(htmlmin({
            removeComments: true, //清除html注释
            collapseWhitespace: true, //压缩html
            collapseBooleanAttributes: true,
            //省略布尔属性的值，例如：<input checked="true"/> ==> <input />
            removeEmptyAttributes: true,
            //删除所有空格作属性值，例如：<input id="" /> ==> <input />
            removeScriptTypeAttributes: true,
            //删除<script>的type="text/javascript"
            removeStyleLinkTypeAttributes: true,
            //删除<style>和<link>的 type="text/css"
            minifyJS: true, //压缩页面 JS
            minifyCSS: true, //压缩页面 CSS
            minifyURLs: true  //压缩页面URL
        }))
        .pipe(gulp.dest('./public'))
});

// 运行gulp命令时依次执行以下任务
gulp.task('default', gulp.parallel(
  'compress', 'minify-css', 'minify-html'
))
```

- 在每次运行完 `hexo generate` 生成静态页面后，运行 `gulp` 对其进行压缩。指令流程如下：

```
PLAINTEXT
hexo clean
hexo generate
gulp
hexo server 或 hexo deploy
```

- 最后自动部署的朋友们确保本地运行成功后在`workflows`中添加`依赖`随后添加`gulp`触发压缩。
  如图所示：
  [![img](https://cdn.jsdelivr.net/gh/L-20021213/picture@1.0.2/img/20210718003125.png)](https://cdn.jsdelivr.net/gh/L-20021213/picture@1.0.2/img/20210718003125.png)