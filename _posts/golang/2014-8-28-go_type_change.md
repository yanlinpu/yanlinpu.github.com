---
layout: post
title: golang类型转换
category : rails
tagline: 原创
tags : [golang, 类型转换]
---
{% include JB/setup %}

<!--{% include themes/custom-settings/time.html %}-->

####strconv包中的函数和方法
    //ParseBool 将字符串转换为布尔值
    //它接受真值：1, t, T, TRUE, true, True
    //它接受假值：0, f, F, FALSE, false, False
    func ParseBool(str string) (value bool, err error)

    //FormatBool 将布尔值转换为字符串 "true" 或 "false"
    func FormatBool(b bool) string


    //ParseFloat 将字符串转换为浮点数
    //s：要转换的字符串 bitSize：指定浮点类型（32:float32、64:float64）
    func ParseFloat(s string, bitSize int) (f float64, err error)


    //ParseInt 将字符串转换为 int 类型
    // s：要转换的字符串  base：进位制（2 进制到 36 进制） bitSize：指定整数类型（0:int、8:int8、16:int16、32:int32、64:int64）
    func ParseInt(s string, base int, bitSize int) (i int64, err error)

    // ParseUint 功能同 ParseInt 一样，只不过返回 uint 类型整数
    func ParseUint(s string, base int, bitSize int) (n uint64, err error)

    // Atoi 相当于 ParseInt(s, 10, 0)  通常使用这个函数，而不使用 ParseInt
    func Atoi(s string) (i int, err error)


    // FormatFloat 将浮点数 f 转换为字符串值
    // f：要转换的浮点数  fmt：格式标记（b、e、E、f、g、G） prec：精度（数字部分的长度，不包括指数部分） bitSize：指定浮点类型（32:float32、64:float64）
    //
    // 格式标记：
    // 'b' (-ddddp±ddd，二进制指数)
    // 'e' (-d.dddde±dd，十进制指数)
    // 'E' (-d.ddddE±dd，十进制指数)
    // 'f' (-ddd.dddd，没有指数)
    // 'g' ('e':大指数，'f':其它情况)
    // 'G' ('E':大指数，'f':其它情况)
    // 如果格式标记为 'e'，'E'和'f'，则 prec 表示小数点后的数字位数
    // 如果格式标记为 'g'，'G'，则 prec 表示总的数字位数（整数部分+小数部分）
    func FormatFloat(f float64, fmt byte, prec, bitSize int) string

    func FormatInt(i int64, base int) string

    func FormatUint(i uint64, base int) string

    // Itoa 相当于 FormatInt(i, 10)
    func Itoa(i int) string

####fmt包中的函数和方法
    //Sprint 将参数列表 a 中的各个参数转换为字符串格式并返回  各个参数之间以空格分隔
    func Sprint(a ...interface{}) string

    // Sprintf 将参数列表 a 填写到格式字符串 format 的占位符中
    func Sprintf(format string, a ...interface{}) string



####参考

* [strconv包](http://www.cnblogs.com/golove/p/3262925.html)

* [fmt包](http://www.cnblogs.com/golove/p/3286303.html)
