---
layout: post
title: golang上传和下载
category : rails
tagline: 参考
tags : [aliyun]
---
{% include JB/setup %}

<!--{% include themes/custom-settings/time.html %}-->

####[oss-aliyun-go对example做了写补充](https://github.com/PinIdea/oss-aliyun-go)

    package main

    import (
        "log"
        "github.com/Pinidea/oss-aliyun-go"
        "os"
        "io/ioutil"
    )

    func main() {
        var (
            accessId  = "z6HPq5j2aoqzAcbt"
            accessKey = "cunI67S8o5s5fvA9myvWkAayTUXhuI"
        )

        // region list:
        // HangZhou         = "oss-cn-hangzhou"
        // QingDao          = "oss-cn-qingdao"
        // HangZhouInternal = "oss-cn-hangzhou-internal"
        // QingdaoInternal  = "oss-cn-qingdao-internal"
        // DefaultRegion    = "oss"
        // 补充其他地方例如北京需要自定义
        // 例如  o := oss.New("oss-cn-beijing", accessId, accessKey)
        o := oss.New(oss.DefaultRegion, accessId, accessKey)

        // get a bucket instance
        bucket := o.Bucket("pinidea-test")

        // upload
        //var data = []byte("this is a test")
        //err := bucket.Put("test.txt", data, "text/plain", oss.PublicRead)

        //upload png
        path := "/home/a.png"
        if PathExist(path){
            f, _ := os.OpenFile(path, os.O_RDONLY, 0666)
            data, _ := ioutil.ReadAll(f)  //序列化
            err := xmlBucket.Put(uuid+".xml", data , "image/png", oss.PublicRead)
            if err != nil {
                panic(err)
            }
            log.Println("success upload object")
        }

        // get object
        text, err := bucket.Get("test.txt")
        if err != nil {
            panic(err)
        }

        log.Println("get object content:", string(text))

        // delete object
        err = bucket.Del("test.txt")
        if err != nil {
            panic(err)
        }
        log.Println("success delete object")
    }
    //判断文件是否存在
    func PathExist(_path string) bool {
    	_, err := os.Stat(_path)
    	if err==nil{
    		return true
    	}
    	return false
    }