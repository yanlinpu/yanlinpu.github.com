---
layout: post
title: 移动端页面布局
category : html
tagline: 原创
tags : [html, mobile-layout]
---
{% include JB/setup %}

<!--{% include themes/custom-settings/time.html %}-->

##布局

    <!DOCTYPE html>
    <html style="overflow-y:scroll;">
    <head>
      <title>移动端页面布局</title>
      <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
      <meta name="viewport" content="width=device-width,minimum-scale=1.0,maximum-scale=1.0"/>
      <meta name="apple-touch-fullscreen" content="no">
      <meta content="telephone=no" name="format-detection">
      <meta name="apple-mobile-web-app-capable" content="no">
    </head>
    <body>

    ......

    </body>
    </html>

##jquery_mobile <error loading page>

单个HTML文档可以包含多个'page',只需要在一个页面包含 多个data-role="page"的div即可,每个pagediv必须由一个唯一的ID (id="foo") ,而链接到相应页面使用锚记即可(href="#foo").当点击一个链接时, 框架会寻找id为锚记href的内部'page'并显示到当前界面中.

要注意如果你正在通过ajax从一个mobile页面链接到一个含有多个内部页面的页面,你需要为该链接添加一个 rel="external" 或者 data-ajax="false" . 该属性告知框架对页面进行重新加载 ,url hash也将清零.这点十分关键,因为ajax 页面使用 hash(#)来追踪ajax历史,当含有多个内部page的页面使用hash 来指示内部page时会发生冲突.

    //后台redirect_to 跳转其他页面时
    <a href="/weixins/login" data-role="button" data-ajax="false" data-inline="true">登陆</a>
    //一个指向含有多个内部page的页面的链接
    <a href="multipage.html" rel="external">Multi-page link</a>