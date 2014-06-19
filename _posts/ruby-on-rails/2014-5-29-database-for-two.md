---
layout: post
title: 第二个数据库的链接
category : rails
tagline: 原创
tags : [rails, 数据库]
---
{% include JB/setup %}

<!--{% include themes/custom-settings/time.html %}-->

##1.需要配置数据库的参数

    database.yml

    two_database:
        host: 192.168.1.110
        port: 8400
        adapter: postgresql
        encoding: unicode
        database: databaseName
        pool: 5
        username: ***
        password: ***

##2.在model中链接数据库

    models/user_two.rb

    class UserTwo < ActiveRecord::Base
        attr_accessible :name,:friends ...
        self.table_name = "user"
        establish_connection "two_database"
    end
