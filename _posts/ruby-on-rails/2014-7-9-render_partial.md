---
layout: post
title: 分页渲染
category : rails
tagline: 原创
tags : [rails, render, paginate]
---
{% include JB/setup %}

<!--{% include themes/custom-settings/time.html %}-->

##以下实现了分页的渲染，包括分页和模糊查询的ajax

    #首页index.html.erb js文件
    <div id="render_area"></div>
    <script>
    $(function(){
        $.ajax({
            type:'post',
            url:'/controller_name/action_name',
            success: function(data){
                $('#render_area').html(data);
            }
        });
    });
    </script>


    #routes.rb
    post '/controller_name/action_name', to: 'controller_name#action_name'


    #controller_name_controller.rb
    def action_name
        city ||= params[:city]
        @objs = ControllerName.where('city like ? ',"%#{city}%").paginate(:per_page => 10, :page => params[:page])
        render partial: 'controller_name/data'
    end


    #views/controller_name/_data.html.erb
    <table>
      <tr>
        <td>CITY<input type="text" id="city"></td>
        <td><input type="button" value="查询" id="select"></td>
      </tr>
    </table>
    <table>
      <tr>
        <th>CITY</th>
        <th>..</th>
        <th>..</th>
        <th>..</th>
      </tr>
      <% @objs.each do |d|%>
        <tr>
          <td><%= d.city %></td>
          <td><%= ... %></td>
          <td><%= ... %></td>
          <td><%= ... %></td>
        </tr>
      <% end %>
    </table>
    <%= will_paginate @objs,:previous_label => '<', :next_label => '>',:class=>'page'%>
    //模糊查询以及异步分页查询
    <script>
        $(function(){
            $('.pagination li a').click(function () {
                event.preventDefault();
                var url = '/controller_name/action_name', href = $(this).context.href, arr = [], page;
                if (!$(this).parent().hasClass('active') && url.indexOf('#') == -1 && href.split('page=').length > 1){
                    page = parseInt(href.split('page=')[1]);
                    $.ajax({
                        type: 'post',
                        url: url,
                        data: {
                            page: page
                        },
                        success: function (data) {
                            $('#render_area').html('');
                            $('#render_area').html(data);
                        }
                    });
                }
            });
            $('#select').click(function(){
                $.ajax({
                    type: 'post',
                    url: '/file_data/get_data',
                    data: {
                        city: $('#city').val()
                    },
                    success: function (data) {
                        $('#render_area').html('');
                        $('#render_area').html(data);
                    }
                });
            });
        })
    </script>