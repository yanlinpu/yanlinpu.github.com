---
layout: post
title: go_workers(sidekiq)
category : golang
tagline: 原创
tags : [go_workers]
---
{% include JB/setup %}

<!--{% include themes/custom-settings/time.html %}-->

[go-workers github](https://github.com/jrallison/go-workers)

方法

    //gsync/workers/user.go

    package workers

    import (
    	"github.com/jrallison/go-workers"
    	"fmt"
    )

    func AddUser(m *workers.Msg){
    	fmt.Println("ADD User")
    	fmt.Println(m.Args().MustArray())
    }
    func UpdateUser(m *workers.Msg){
    	fmt.Println("UPDATE User")
    }

worker创建

    //gsync/worker.go

    package main

    import (
    	"github.com/jrallison/go-workers"
    	w "gsync/workers"
    )


    type myMiddleware struct{}

    func (r *myMiddleware) Call(queue string, message *workers.Msg, next func() bool) (acknowledge bool) {
    	// do something before each message is processed
    	acknowledge = next()
    	// do something after each message is processed
    	return
    }

    func main() {
    	workers.Configure(map[string]string{
    		// location of redis instance
    		"server":  "localhost:6379",
    		// instance of the database
    		"database":  "0",
    		// number of connections to keep open with redis
    		"pool":    "30",
    		// unique process id for this instance of workers (for proper recovery of inprogress jobs on crash)
    		"process": "1",
    	})

    	workers.Middleware.Append(&myMiddleware{})

    	// pull messages from "add_patient" with concurrency of 10
    	workers.Process("add_patient", w.AddUser, 10)

    	// pull messages from "update_patient" with concurrency of 20
    	workers.Process("update_patient", w.UpdateUser, 20)


    	// stats will be available at http://localhost:8080/stats
        // go workers.StatsServer(8080)

    	// Blocks until process is told to exit via unix signal
    	workers.Run()
    }

build 并执行

`$go build worker.go`

`$nohup ./worker &`

加入队列

    //action.go

    package main

    import (
    	"github.com/jrallison/go-workers"
    )
    func main(){
    	workers.Configure(map[string]string{
    		// location of redis instance
    		"server":  "localhost:6379",
    		// instance of the database
    		"database":  "0",
    		// number of connections to keep open with redis
    		"pool":    "30",
    		// unique process id for this instance of workers (for proper recovery of inprogress jobs on crash)
    		"process": "1",
    	})
    	workers.Enqueue("add_patient","add_patient",[]string{"张三","北京市","15088888888"})
    }

`$go run action.go`