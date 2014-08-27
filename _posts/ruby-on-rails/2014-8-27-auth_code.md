---
layout: post
title: rmagick生成验证码
category : rails
tagline: 原创
tags : [rails, 验证码, rmagick]
---
{% include JB/setup %}

<!--{% include themes/custom-settings/time.html %}-->

##用rmagick生成验证码，并实现点击刷新

####1.Gemfile文件
    gem "rmagick"

####2.model/noisy_image.rb
    require 'rubygems'
    require 'RMagick'
    class NoisyImage
      include Magick
      attr_reader :code, :code_image
      Jiggle = 15
      Wobble = 15
      def initialize(len=4)
        chars = ('a'..'z').to_a - ['a','e','i','o','u']
        code_array=[]
        1.upto(len) {code_array << chars[rand(chars.length)]}
        granite = Magick::ImageList.new('xc:#EDF7E7')
        canvas = Magick::ImageList.new
        canvas.new_image(32*len, 50, Magick::TextureFill.new(granite))
        text = Magick::Draw.new
        text.font_family = 'times'
        text.pointsize = 40
        cur = 10

        code_array.each{|c|
          rand(10) > 5 ? rot=rand(Wobble):rot= -rand(Wobble)
          rand(10) > 5 ? weight = NormalWeight : weight = BoldWeight
          text.annotate(canvas,0,0,cur,30+rand(Jiggle),c){
            self.rotation=rot
            self.font_weight = weight
            self.fill = 'green'
          }
          cur += 30
        }
        @code = code_array.to_s
        @code_image = canvas.to_blob{
          self.format="JPG"
        }
      end

    end

####3.controller/code_controller.rb
    class CodeController < ActionController::Base
      def code_image
        @noisy = NoisyImage.new(4) #生成一个有4字符的图片
        @code=""
        @code.insert(0, @noisy.code[2])
        @code.insert(1, @noisy.code[7])
        @code.insert(2, @noisy.code[12])
        @code.insert(3, @noisy.code[17])
        session[:code] = @code
        send_data @noisy.code_image, :type => 'image/jpeg', :disposition => 'inline'
      end
    end

####4.routes.rb
    resource :code do
        collection do
          get '/code_image' => 'code#code_image'
        end
    end

####5.view 层调用生成验证码图片
    <img src="/code/code_image" id="code_img" onclick="changeImage()"/>
    <script>
        function changeImage(){
            $("#code_img").attr("src","/code/code_image?tmp="+new Date().getTime()); //+tmp是为了避免缓存
        }
    </script>

####6.小结

* [参考](http://www.blogjava.net/chengang/archive/2007/08/17/137683.html)

* script +tmp是为了避免缓存

* session不要存储图片，否则会产生[502 Bad Gateway nginx/1.1.19](https://ruby-china.org/topics/13877)这个错误