---
title: 404
permalink: /404
date: 2022-04-07 12:54:38
comments: false
layout: false
---

<!DOCTYPE HTML>
<html>
<head>
  <meta http-equiv="content-type" content="text/html;charset=utf-8;"/>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
  <meta name="robots" content="all" />
  <meta name="robots" content="index,follow"/>
<style>
    *{
        margin: 0;
        padding: 0;
    }
    .iframe{
        width: 100%; height: 450px; border: 0; overflow: hidden; position: relative;
    }
    a{
        color: #ffaa00;
        padding: 0 8px;
        font-size: 14px;
    }
</style>
</head>
<body>
<div class="iframe">
    <iframe id="iframeSrc" style="width: 100%; height: 450px; border: 0; overflow: hidden;" scrolling="no"></iframe>
    <a href="/" style="width: 230px; height: 60px; position: absolute; top: 125px; left: 50%; transform: translateX(-50%); background: transparent"></a>
</div>

<div class="content-404" style="text-align: center">
        <h1 style="font-size: 60px; position: relative; top: 85px; color: #f18209">4<span style="color: #ffaa00">0</span>4</h1>
        <img src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-b239efaa-5152-4c7c-a688-7f7519bc8433/be286ccb-6d75-449a-a451-feb35c2c06dc.png" alt="404" style="width: 300px">
        <p style="font-size: 14px; padding-bottom: 15px">抱歉，您访问的页面不存在</p>
        <a href="/">返回首页</a> <a href="javascript:history.back(-1)">返回上一页</a>
</div>
</body>
<script>
    document.querySelector("#iframeSrc").setAttribute("src", location.origin);
    document.getElementById("iframeSrc").scrolling="no";
</script>
</html>
