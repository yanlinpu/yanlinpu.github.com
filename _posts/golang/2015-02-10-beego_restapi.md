---
layout: post
title: beego API swagger
category : golang
tagline: 原创
tags : [beego API]
---
{% include JB/setup %}

<!--{% include themes/custom-settings/time.html %}-->

[beego API自动化文档](http://beego.me/blog/beego_api)

一、初始化

`$bee new apidemo`

`$cd apidemo`

`$bee generate docs`

二、修改

- 配置文件
    - 添加
		EnableDocs = true
		copyrequestbody = true（post接口可以接收对象）
    - main.go 引入docs _ "gsandbox/beego/apidemo/docs"
	- mian.go 加入swagger

		    if beego.RunMode == "dev" {
			    beego.DirectoryIndex = true
			    beego.StaticDir["/swagger"] = "swagger"
		    }

三、下载swagger并运行

`$bee run -downdoc=true -gendoc=true`

四、修改swagger

- 删除font 改善加载速度

    swagger/swagger-1/index.html

- 取消主键Id CreatedAt UpdatedAt

    swagger/swagger-1/lib/swagger.js

        if (propertyName=="Id"||propertyName=="CreatedAt"||propertyName=="UpdatedAt"||propertyName=="Guid")
            continue;
        prop = new SwaggerModelProperty(propertyName, obj.properties[propertyName]);
        this.properties.push(prop);

五、rails生成model 用现成的数据库生成restapi

- rails 创建数据库

    `$cd models`

    `$rails new railsbot`

    `$cd railsbot`

    `$rails generate model User name:string address:string home_phone:string`

    `$rake db:create`

    `$rake db:migrate`

- 根据数据库生成beego的 model controller  router（API）

    `bee generate appcode -tables="users" -driver=mysql -conn="root:123@tcp(127.0.0.1:3306)/apidemo" -level=3`

    - tables="users"  根据users表生成代码，默认为空，为空时表示所有的表
    - driver=mysql  数据库驱动，默认为mysql
    - conn="root:123@tcp(127.0.0.1:3306)/apidemo" 数据库连接，用户名：root；密码：123；ip地址：127.0.0.1；端口：3306；数据库名称：apidemo
    - level=3   1表示生成models；2表示生成models,controllers；3表示生成models,controllers,router,默认为3

六、运行

`$cd $GOPATH/src/beegoapi`

`$bee run -gendoc=true`

七、访问

localhost:8080/swagger/swagger-1