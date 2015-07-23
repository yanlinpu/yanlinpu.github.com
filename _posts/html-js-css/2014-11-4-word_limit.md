---
layout: post
title: jquery限制显示字数
category : html
tagline: 原创
tags : [javascript]
---
{% include JB/setup %}

<!--{% include themes/custom-settings/time.html %}-->

##jquery代码如下（考虑中英文）
    <table>
        <tr>
            <td class="test">jquery限制显示字数</td>
        <tr>
    </table>
    <script>
        //获取string总长度
        function fucCheckLength(strTemp) {
            var i,sum;
            sum=0;
            for(i=0;i<strTemp.length;i++) {
                if ((strTemp.charCodeAt(i)>=0) && (strTemp.charCodeAt(i)<=255)) {
                    sum=sum+1;
                }else {
                    sum=sum+2;
                }
            }
            return sum;
        };
        //返回index strTemp在num位置时的index
        function returnIndex(strTemp,num){
            var i,sum;
            sum=0;
            for(i=0;i<strTemp.length;i++) {
                if ((strTemp.charCodeAt(i)>=0) && (strTemp.charCodeAt(i)<=255)) {
                    sum=sum+1;
                }else {
                    sum=sum+2;
                }
                if(sum>num){
                    break;
                }
            }
            return i;
        }
        //...替换
        function replaceNew(obj,num){
            $(obj).each(function(){
                var total_len = fucCheckLength2($(obj).text());
                if (total_len > num) {
                    var text_len = returnIndex2($(obj).text(),num);
                    $(obj).text($(obj).text().substring(0, text_len) + "...");
                }
            })
        }
        $(function () {
            replaceNew($('.test'),9);
        });
    </script>

##原型（不考虑中英文）

    $(function(){
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
    })

##div保留两行其余（...）

    .figcaption{style:'width: 100px; height: 44px; line-height: 22px; overflow: hidden'}
      %p.pro_con{:style=>"word-wrap: break-word;"}
        测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试测试

    :javascript
      $(function()
      {
          $(".figcaption").each(function(i){
              var divH = $(this).height();
              var $p = $("p", $(this)).eq(0);
              while ($p.outerHeight() > divH) {
                 $p.text($p.text().replace(/(\s)*([a-zA-Z0-9]+|\W)(\.\.\.)?$/, "..."));
              };
          });
      });