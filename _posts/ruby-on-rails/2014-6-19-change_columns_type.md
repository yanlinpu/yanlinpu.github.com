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

