---
layout: post
title: 修改PG数据库字段类型（string->integer）
category : rails
tagline: 原创
tags : [rails, 数据库]
---
{% include JB/setup %}

<!--{% include themes/custom-settings/time.html %}-->

##1.生成文件

    rails g migration change_data_type_for_table_column

##2.在生成的文件中需要填写change_column方法

通常会这样写：

    change_column :table_name, :column_name, :integer

但是PostgresSQL会产生如下错误：

    PG::DatatypeMismatch: ERROR:  column "column_name" cannot be cast automatically to type integer

为避免此错误，需修改为：

    class ChangeDataTypeForTableColumn < ActiveRecord::Migration
        def change
            change_column :table_name, :column_name, 'integer USING CAST(report_type AS integer)'
        end
    end

但此方法，不支持其他数据库。比如mysql

    Mysql2::Error: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'USING CAST(report_type AS integer) DEFAULT NULL' at line 1

    change_column :table_name, :column_name, :integer

还有一种方法就是先remove后add

    remove_column :table, :column
    add_column :table, :column, :integer