---
layout: post
title: 表单ajax
category : rails
tagline: 原创
tags : [rails, form, ajax]
---
{% include JB/setup %}

<!--{% include themes/custom-settings/time.html %}-->

##rails自带的form_for ajax

    #views/sessions/login.html.erb
    <div id="content"></div>
    <%= form_for(:session,remote: true,:url => {:action => 'create', :controller=>'sessions' }, :html=>{:id => 'sess_form'}) do |f| %>
      <%= f.label :username,"用户名"%>
      <%= f.text_field :username,:placeholder=>"用户名"%>
      <%= f.label :password,"密码"%>
      <%= f.password_field :password,:placeholder=>"密码"%>
      <%= f.submit '登录'%>
    <% end %>

    #sessions_controller.rb
    def create
      login_name = params[:session][:username]
      password = params[:session][:password]
      @flag={}
      if User.where("name=? and password=?",login_name,password).size>0
        @flag={:flag => true}
      else
        @flag={:flag => false}
      end
      respond_to do |format|
        format.html
        format.json { render json: @flag }
        format.js
      end
    end

    #views/sessions/create.js.erb
    <% if @flag[:flag] %>
        $('#content').html("<script>$(document).ready(function(){ location.href='/home'});</script>");
    <% else %>
        $('#content').html("<script>alert("登录异常，请重新登录！");</script>");
    <% end %>

