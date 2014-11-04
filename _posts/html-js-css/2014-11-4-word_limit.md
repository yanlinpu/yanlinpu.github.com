---
layout: post
title: jquery限制显示字数
category : html
tagline: 原创
tags : [javascript]
---
{% include JB/setup %}

<!--{% include themes/custom-settings/time.html %}-->

##jquery代码如下
    <table>
        <tr>
            <td class="test">jquery限制显示字数</td>
        <tr>
    </table>
    <script>
        $(function () {
            $(".test").each(function () {
                var maxWidth = 6;
                if ($(this).text().length > maxWidth) {
                    var b = $(this).children().is("a");
                    if (b) {
                        $(this).children().text($(this).children().text().substring(0, maxWidth) + "...");
                    } else {
                        $(this).text($(this).text().substring(0, maxWidth));
                        $(this).text($(this).text() + "...");
                    }
                }
            });
        });
    </script>