---
title: 利用gulp打包------解决ES6代码打包出错问题
date: 2022-10-09 09:24:46
tags: ["gulp","babel"]
categories: ["gulp"]
---

在项目中使用gulp对源码及项目进行压缩和打包是很常见的做法，但是实际中可能会遇见uglify执行出错的问题，本文只针对由于项目中使用ES6语法而造成uglify失败的问题。

解决这个问题主要是借助于babel。直接上操作流程。

解决这个问题主要是借助于babel。直接上操作流程。

1、全局安装babel。使用命令 npm install -g babel        和npm install -g babel-cli

2、本地安装gulp-babel。   npm install --save-dev gulp-babel

3、安装babel 辅助插件。 npm install --save-dev babel-preset-env

4、安装babel 辅助插件。 npm install --save-dev babel-core babel-preset-es2015

5、在项目根目录创建文件。.babelrc文件。文件内容

```json
{
  "presets": [
    "es2015"
  ],
  "plugins": []
}
```

6、修改gulpfile.js.关键代码如下

```javascript
var gulp = require('gulp'),
    uglify = require('gulp-uglify'),
    babel = require('gulp-babel');
    rename = require('gulp-rename');
 
 
// 压缩 js 文件
gulp.task('jscompress', function() {
    return gulp.src('src/script/*.js')
        .pipe(babel())
        .pipe(uglify())
        .pipe(rename({suffix:'.min'}))
        .pipe(gulp.dest('dist/script/'))
        ;
});
 
gulp.task('default',['jscompress']);
```

7、这时候运行即可gulp task即可。

但是，你有可能执行任务失败，提示找不到babel-core库，不管你怎么安装都不行。这时候请检查使用的插件版本是否配套。贴出我的版本，如果版本问题，请 npm  uninstall 库   重新    npm install 库@版本即可。


```json
{
    "devDependencies": {
        "babel-core": "^6.26.3",
        "babel-preset-env": "^1.7.0",
        "babel-preset-es2015": "^6.24.1",
        "gulp": "^3.9.1",
        "gulp-babel": "^7.0.1",
        "gulp-rename": "^1.4.0",
        "gulp-uglify": "^3.0.1"
    }
}
```
