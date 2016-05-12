---
layout: post
title: c语言向函数传递函数作为参数
category: C语言
tags: 
keywords: 
description:
---

####	函数

````swift
int aFunc(){
    int value = 2;
    return value;
}
int bFunc(int (*f)()){
    return f();
}
````

####	将函数a传递到b中

````swift
int x =  bFunc(aFunc);
````

####	执行结果
`x = 2`