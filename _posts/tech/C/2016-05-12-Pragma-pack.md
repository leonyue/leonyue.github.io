---
layout: post
title: pragma pack(非常有用的字节对齐用法说明)
category: C语言
tags: 
keywords: 
description:
---

## 介绍
---
程序编译器对结构的存储的特殊处理确实提高CPU存储变量的速度，但是有时候也带来了一些麻烦，我们也屏蔽掉变量默认的对齐方式，自己可以设定变量的对齐方式。
编译器中提供了#pragma pack(n)来设定变量以n字节对齐方式。n字节对齐就是说变量存放的起始地址的偏移量有两种情况：第一、如果n大于等于该变量所占用的字节数，那么偏移量必须满足默认的对齐方式，第二、如果n小于该变量的类型所占用的字节数，那么偏移量为n的倍数，不用满足默认的对齐方式。结构的总大小也有个约束条件，分下面两种情况：如果n大于所有成员变量类型所占用的字节数，那么结构的总大小必须为占用空间最大的变量占用的空间数的倍数；否则必须为n的倍数。


##  测试代码
---

````swift
//
//  main.m
//  testC
//
//  Created by YC-JG-YXKF-PC35 on 16/5/10.
//  Copyright © 2016年 yuneec. All rights reserved.
//

#import <Foundation/Foundation.h>

#pragma pack(push)
#pragma pack(1)

struct test
{
    char x1;
    short x2;
    float x3;
    char x4;
};
#pragma pack(pop)

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        // insert code here...

        struct test a;
        a.x1 = 'z';
        a.x2 = 11112;
        a.x3 = -16.17;
        a.x4 = 'z';
        NSLog(@"size of a:%lu",sizeof(a));


        char *b = malloc(sizeof(a));
        memcpy(b, &a, sizeof(a));
        NSData *data = [NSData dataWithBytes:b length:sizeof(a)];
        int n=1;
        printf(*(char *)&n ? "小端\n" : "大端\n");
        char x1 = b[0];
//        short x2 = (b[2] << 8) + b[1]; //左移2^8 一个字节 小端低地址低位
        short x2;
        memcpy(&x2, b+1, sizeof(short));
        float x3;
        memcpy(&x3, b+3, sizeof(float));

//        *(char *)&x3 = *(b+3);
//        *x3f = 0xae;
//        *(x3f+1)=0x47;
//        *(x3f+2)=0x81;
//        *(x3f+3)=0xc1;




        char x4 = 0x7a;
        free(b);
        NSLog(@"Hello, World!");


    }
    /*
     1字节
    <7a682b33 3333417a>
     自然
    <7a00682b 33333341 7a000000>
     */
    return 0;
````