---
layout: post
title: nokogiri(xpath解析xml)
category : rails
tagline: 原创
tags : [nokogiri]
---
{% include JB/setup %}

<!--{% include themes/custom-settings/time.html %}-->

##[w3c xpath的讲解](http://www.w3school.com.cn/xpath/xpath_syntax.asp)

    <?xml version="1.0" encoding="ISO-8859-1"?>

    <bookstore>

    <book>
      <title lang="eng">Harry Potter</title>
      <price>29.99</price>
    </book>

    <book>
      <title lang="eng">Learning XML</title>
      <price>39.95</price>
    </book>

    </bookstore>

####最有用的路径表达式

| 表达式 | 描述 |
|-------+-----|
| nodename | 选取此节点的所有子节点。 |
| / | 从根节点选取。 |
| // | 从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置。 |
| . | 选取当前节点。 |
| .. | 选取当前节点的父节点。 |
| @ | 选取属性。 |

####实例

| 路径表达式 | 结果 |
|-------+-----|
| bookstore | 选取 bookstore 元素的所有子节点。 |
| /bookstore | 选取根元素 bookstore。注释：假如路径起始于正斜杠( / )，则此路径始终代表到某元素的绝对路径！ |
| bookstore/book | 选取属于 bookstore 的子元素的所有 book 元素。 |
| //book | 选取所有 book 子元素，而不管它们在文档中的位置。 |
| bookstore//book | 选择属于 bookstore 元素的后代的所有 book 元素，而不管它们位于 bookstore 之下的什么位置。 |
| //@lang | 选取名为 lang 的所有属性。 |
|-------+-----|
| /bookstore/book[1] | 选取属于 bookstore 子元素的第一个 book 元素。 |
| /bookstore/book[last()] | 选取属于 bookstore 子元素的最后一个 book 元素。 |
| /bookstore/book[position()<3] | 选取最前面的两个属于 bookstore 元素的子元素的 book 元素 |
| //title[@lang] | 选取所有拥有名为 lang 的属性的 title 元素。 |
| //title[@lang='eng'] | 选取所有 title 元素，且这些元素拥有值为 eng 的 lang 属性。 |
| /bookstore/book[price>35.00] | 选取 bookstore 元素的所有 book 元素，且其中的 price 元素的值须大于 35.00。 |
| /bookstore/book[price>35.00]/title | 选取 bookstore 元素中的 book 元素的所有 title 元素，且其中的 price 元素的值须大于 35.00。 |
| /bookstore/* | 选取 bookstore 元素的所有子元素。 |
| //* | 选取文档中的所有元素。 |
| //title[@*] | 选取所有带有属性的 title 元素。 |

##Nokogiri
    ##XML
    <?xml version="1.0" encoding="UTF-8"?>

    <bookstore>

      <book>
        <author name="me">J K. Rowling</author>
        <year>2005</year>
        <price>29.99</price>
      </book>

    </bookstore>

    require 'nokogiri'
    #如果是服务器文件需要open-uri
    require 'open-uri'

    path = "/home/111.xml"
    xml = Nokogiri::XML.parse(File.read(path), path)
    #如果是服务器xml文件
    xml = Nokogiri::XML(open(path))
    #获取属性值value
    xml.xpath("//book/author").attr("name").to_s
    xml.xpath("/bookstore/book/author").attr("name").text
    xml.xpath("bookstore/book/author").attribute("name").to_s
    xml.xpath("//book/author").attribute("name").text

    #获取节点值 text
    xml.xpath("//book/author").text

    #to_s 输出值为"<author name=\"me\">J K. Rowling</author>"
    xml.xpath("//book/author").to_s
