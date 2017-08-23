---
layout: post
title: C++：private继承与public继承
category: Cplusplus
tags: 
keywords: 
description:
---


1.	private, public, protected 访问标号的访问范围

private：只能由1.该类中的函数、2.其友元函数访问。

不能被任何其他访问，该类的对象也不能访问。

protected：可以被1.该类中的函数、2.子类的函数、3.其友元函数访问。

但不能被该类的对象访问。

public：可以被1.该类中的函数、2.子类的函数、3.其友元函数访问，也可以由4.该类的对象访问。

注：友元函数包括3种：设为友元的普通的非成员函数；设为友元的其他类的成员函数；设为友元类中的所有成员函数。

2.	类被继承后方法属性变化
private 属性不能够被继承。
使用private继承， 父类的protected和public属性在子类中变为private；
使用protected继承，父类的protected和public属性在子类中变为protected；
使用public继承， 父类的protected和public属性不发生改变;

3.	private继承和public继承的适用情况
C++将public继承视为is-a关系。private继承则并不意味着is-a关系，private继承意味着implemented-in-terms-of（根据某物实现出）。private继承意味着只有实现部分被继承，接口部分被略去。private继承在软件设计层面上没有意义，其意义只在于软件实现层面。

private继承：

1)	编译器不会自动将一个子类对象转换为一个父类对象，而public继承会；

2)	子类中由父类继承而来的成员（protected和public）都变为private。

implemented-in-terms-of也可以由复合实现。在应用域，复合意味着has-a；在实现域，复合意味着is-implemented-in-terms-of。尽可能使用复合实现这种关系，必要时（涉及protected成员或virtual函数时）才使用private继承。