---
layout: post
title: beego 访问远程数据库
category : golang
tagline: 原创
tags : [orm]
---
{% include JB/setup %}

<!--{% include themes/custom-settings/time.html %}-->

##Mysql

    dbHost := "***.***.***.***"
    dbPort := "3306"
    dbUser := "root"
    dbPass := "123456"
    dbName := "fit_production"
    dbDSN:=fmt.Sprintf("%s:%s@tcp(%s:%s)/%s?charset=utf8",dbUser,dbPass,dbHost,dbPort,dbName)
    orm.RegisterDataBase("default","mysql",dbDSN)

##postgresql sqlite3

    dbDSN := "dbname=fit_production user=root password=123456 host=111.111.111.111 port=8400 sslmode=disable"
    orm.RegisterDataBase("pg", "postgres", dbDSN , 30)


    orm.RegisterDataBase("sqlite", "sqlite3", conf.LocalDb)

