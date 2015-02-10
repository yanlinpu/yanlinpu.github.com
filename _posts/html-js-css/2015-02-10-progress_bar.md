---
layout: post
title: 简单的进度条
category : html
tagline: 原创
tags : [javascript]
---
{% include JB/setup %}

<!--{% include themes/custom-settings/time.html %}-->

##demo如下

    <style>
        #center{
            margin:50px auto;
            width:400px;
        }
        #loading{
            width:397px;
            height:49px;
        }
        #loading div{
            width:0px;
            height:48px;
            background-color: #27B9B3;
            color:#fff;
            text-align:center;
            font-family:Tahoma;
            font-size:18px;
            line-height:48px;
        }
    </style>


    <div id="center">
        <div id="message"></div>
        <div id="loading"><div></div></div>
    </div>

    <script type="text/javascript">
        function SetProgress(progress) {
            if (progress) {
                $("#loading > div").css("width", String(progress) + "%"); //控制#loading div宽度
                $("#loading > div").html(String(progress) + "%"); //显示百分比
            }
        }
        var i = 0;
        function doProgress() {
            if (i > 100) {
                $("#center").css("visibility","hidden");//加载完毕隐藏
            }else{
                setTimeout("doProgress()", 100);
                SetProgress(i);
                i++;
            }
        }
        $(document).ready(function() {
            doProgress();
        });
    </script>



