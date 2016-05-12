---
layout: post
title: 负数补码运算
category: C语言
tags: 
keywords: 
description:
---

####	补码计算

1.	正数的补码：与原码相同。  
【例1】+9的补码是00001001。  
2.	负数的补码：符号位为1，其余位为该数绝对值的原码按位取反；然后整个数加1。  
【例2】求-7的补码。   
	因为给定数是负数，则符号位为“1”。  
后七位：+7的原码（0000111）→按位取反（1111000）→加1（1111001）  
所以-7的补码是11111001。
	
####	补码减法    
[X-Y]补 = [X]补 - [Y]补 = [X]补 + [-Y]补  
其中[-Y]补称为负补，求负补的方法是：所有位（包括符号位）按位取反；然后整个数加1。  
*****以上内容来自百度知道******  
按照补码减法的规则：  
[-54-30]补 = [-54]补 + [-30]补  
-54的补码：因为是负数，所以符号位为1，54=32+16+4+2=0110110(2),取反=1001001，加1=1001010，所以-54的补码是1 1001010.  
同理,30=16+8+4+2=0011110(2),取反=1100001，加1=1100010，-30的补码是1 1100010.  
[-54-30]补=1 1001010 + 1 1100010 = 1 0101100  
根据补码的补码是原码：  
[[-54-30]补]补=原码  
符号位为1，说明为负数，0101100取反=1010011，加1=1010100  
转化为10进制，得84  
故结果为-84