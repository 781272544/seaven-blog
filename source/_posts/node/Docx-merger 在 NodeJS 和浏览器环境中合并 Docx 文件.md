---
title: Docx-merger | 在 NodeJS 和浏览器环境中合并 Docx 文件(即world文件)
date: 2023-07-20 10:30:27
tags: ["node","Docx-merger","生产world"]
categories: ["node"]
---

## 一、搭建项目运行环境

1.项目初始化

```
npm init -y
```

2.安装docx-merger插件依赖

```
npm install docx-merger
```

## 二、代码

1.JS代码 

Docx-merger的核心代码 

```js

var DocxMerger = require('./docx-merger');
 
var fs = require('fs');
var path = require('path');
 
var file1 = fs.readFileSync(path.resolve(__dirname, 'template.docx'), 'binary');
 
var file2 = fs.readFileSync(path.resolve(__dirname, 'template1.docx'), 'binary');
 
var docx = new DocxMerger({}, [file1, file2]);
 
docx.save('nodebuffer', function (data) {
    fs.writeFile("output.docx", data, function (err) {/*...*/ });
});
```

2.HTML代码

```html

<html>
<script src="./node_modules/docx-merger/dist/docx-merger.min.js"></script>
<script src="https://cdn.bootcdn.net/ajax/libs/FileSaver.js/2.0.5/FileSaver.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jszip-utils/0.0.2/jszip-utils.min.js"></script>
 
<script>
    function loadFile(url, callback) {
 
        return new Promise(function (resolve, reject) {
 
            JSZipUtils.getBinaryContent(url, function (err, data) {
                if (err) reject(err);
                resolve(data);
            });
        });
    }
    Promise.all([loadFile("template.docx"), loadFile("template1.docx")]).then(function (files) {
 
        var docx = new DocxMerger({}, files);
 
        docx.save('blob', function (data) {
            saveAs(data, "output.docx");
        });
    }, function (err) {
        alert(err);
    })
</script>
 
</html>
```

2.2  promise.html

```html
<html>
<script src="./node_modules/docx-merger/dist/docx-merger.min.js"></script>
<script src="https://cdn.bootcdn.net/ajax/libs/FileSaver.js/2.0.5/FileSaver.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jszip-utils/0.0.2/jszip-utils.min.js"></script>
 
<script>
    function loadFile(url, callback) {
        JSZipUtils.getBinaryContent(url, callback);
    }
    loadFile("template.docx", function (error, file1) {
        loadFile("template1.docx", function (error, file2) {
 
            var docx = new DocxMerger({}, [file1, file2]);
 
            docx.save('blob', function (data) {
                saveAs(data, "output.docx");
            });
        })
    })
</script>
 
</html>
```
