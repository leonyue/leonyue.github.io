---
layout: post
title: static inline 和 extern inline 的含义
category: iOS
tags: iOS
keywords: iOS
description:
---


问：

首先，关于inline就够烦人了，有的书上说inline关键字要加在定义前，声明时可以省略，有的说声明时加上inline函数就变成内联型， 有的说声明和定义形式要保持一致。在一个类中声明一个函数，函数的实现在外部，无论是仅仅在内部声明处加inline，还是在外部实现处加inline， 或是两个地方都加，编译均能通过，而且也无法通过调试的办法看出对程序到底有啥影响。搞不清到底要怎么写这个inline才比较好，不过可以肯定的 是，inline函数的定义部分要放在头文件里，声明和定义分开放会编译出错。 

而且inline还可以和extern关键字、static关键字合用，在网上搜了一下，linux之父linus说过 "static inline" means "we have to have this function, if you use it, but don't inline it, then make a static version of it in this compilation unit". "extern inline" means "I actually _have_ an extern for this function, but if you want to inline it, here's the inline-version". 
这话说的云里雾里的，谁能解释一下，说说你对static inline 和 extern inline用法的理解。

答：

extern inline表示该函数是已声明过的了.由于函数本身可以声明多次,所以extern对函数的影响仅仅把函数的隐藏属性显式化了. 

extern 对于非函数的对象是有用的,因为对象声明时会带来内存的分配,而用 extern就表示该对象已经声明过了,不用再分配内存. 

static是以前C的用法.目的是让该关键字标识的函数只在本地文件可见,同一个程序的其它文件是不可见该函数的.换句话说,就算你其它文件里包含了同名同参数表的函数定义的话,也是不会引起函数重复定义的错误的.因为static是仅在当前文件可见. 

关于inline函数,你说的大部分的都 是对的.我来给你总结一下吧. 

inline函数仅仅是一个建议,对编译器的建议,所以最后能否真正内联,看编译器的意思,它如果认为你的函数不复杂,能在调用点展开,就会真正内联,并不是说声明了内联就会内联,你声明内联只是一个建议而已. 

其次,因为内联函数要在调用点展开,所以编译器必须随处可见内联函数的定义,要不然,就成了非内联函数的调用了.所以,这要求你的每个调用了内联函数的文 件都出现了该内联函数的定义,因此,将内联函数放在头文件里实现是合适的,省却你为每个文件实现一次的麻烦.而你所以声明跟定义要一致,其实是指,如果你 在每个文件里都实现一次该内联函数的话,那么,你最好保证每个定义都是一样的,否则,将会引起未定义的行为,即是说,如果不是每个文件里的定义都一样,那 么,编译器展开的是哪一个,那要看具体的编译器而定.所以,最好将内联函数定义放在头文件中. 

而类中的成员函数缺省都是内联的,如果你在类定义时就在类内给出函数,那当然最好.如果你在类中未给出成员函数定义,而你又想内联该函数的话,那在类外要 加上inline,否则就认为不是内联的.而且刚说了,内联函数最好放在头文件内,所以最好在类定义的头文件里把类的内联函数都实现了. 

而你说的将声明与实现分开,其实是不会编译出错的,反正我写那么多程序都没试过.将声明与定义分开的话,这样的后果会带来编译器并不随处可见该函数定义,所以,只能在你实现定义的那个文件里,将该函数看成内联(如果可以内联的话),在其它文件,仍看成是普通函数. 

看到这里,我想你应该明白了.那么声明时加inline,实现时要不要加inline呢?呵呵,留给 lz 思考吧. 

    extern inline表示该函数是已声明过的了.由于函数本身可以声明多次,所以extern对函数的影响仅仅把函数的隐藏属性显式化了. 
    
extern 对于非函数的对象是有用的,因为对象声明时会带来内存的分配,而用 extern就表示该对象已经声明过了,不用再分配内存. 

static是以前C的用法.目的是让该关键字标识的函数只在本地文件可见,同一个程序的其它文件是不可见该函数的.换句话说,就算你其它文件里包含了同名同参数表的函数定义的话,也是不会引起函数重复定义的错误的.因为static是仅在当前文件可见. 

关于inline函数,你说的大部分的都 是对的.我来给你总结一下吧. 

inline函数仅仅是一个建议,对编译器的建议,所以最后能否真正内联,看编译器的意思,它如果认为你的函数不复杂,能在调用点展开,就会真正内联,并不是说声明了内联就会内联,你声明内联只是一个建议而已. 

其次,因为内联函数要在调用点展开,所以编译器必须随处可见内联函数的定义,要不然,就成了非内联函数的调用了.所以,这要求你的每个调用了内联函数的文 件都出现了该内联函数的定义,因此,将内联函数放在头文件里实现是合适的,省却你为每个文件实现一次的麻烦.而你所以声明跟定义要一致,其实是指,如果你 在每个文件里都实现一次该内联函数的话,那么,你最好保证每个定义都是一样的,否则,将会引起未定义的行为,即是说,如果不是每个文件里的定义都一样,那 么,编译器展开的是哪一个,那要看具体的编译器而定.所以,最好将内联函数定义放在头文件中. 

而类中的成员函数缺省都是内联的,如果你在类定义时就在类内给出函数,那当然最好.如果你在类中未给出成员函数定义,而你又想内联该函数的话,那在类外要 加上inline,否则就认为不是内联的.而且刚说了,内联函数最好放在头文件内,所以最好在类定义的头文件里把类的内联函数都实现了. 

而你说的将声明与实现分开,其实是不会编译出错的,反正我写那么多程序都没试过.将声明与定义分开的话,这样的后果会带来编译器并不随处可见该函数定义,所以,只能在你实现定义的那个文件里,将该函数看成内联(如果可以内联的话),在其它文件,仍看成是普通函数. 

看到这里,我想你应该明白了.那么声明时加inline,实现时要不要加inline呢?呵呵,留给 lz 思考吧.

 

 

呵呵,你看的还是英文版的哦.你理解的不是这样,放在头文件中不是只存在一个定义,而是只要包含了该头文件的程序文本文件都存在了这个定义.而inline函数是可以被重复定义的,在C++中,常量对象跟内联函数都是可以多次定义的. 
你要把函数那章都看完就会明白. 

我先假设你的函数符合内联的条件. 

在声明是加inline,定义时不加,则要求编译器编译时,能看到inline的声明,而且在展开点看到该定义,这样,就将其视为内联函数. 

如果你声明没有inline,却在定义时inline了.这时,如果其它要调用该函数的文件看到了它的声明,就认为该函数不是内联的,所以,到了调用处, 转到该函数实现的地方,却意外地看到了inline声明,这时,会导致链接出错.若要改正的话,就要让调用该函数的文件也看到有inline的定义,而不 是在调用时才看到.你可以在每个文件都加上有inline的定义.(如果不加inline,则会出现重复定义的错误,因为内联函数才可以被重复定义).或 者另一种修改方法,你将定义时的inline去掉,这样就成为普通函数,链接不会出错.如果是前一种改法,仍是内联的,因为符合了看到了inline且随 处可见其定义的条件. 
如果你将声明跟定义都放在同一个头文件,而在声明时不内联,在实现时内联,这样编译器也是将该函数内联(符合两个条件,看到inline的声明(虽然是在定义时),随处可见其定义). 

总结说来,只要编译器看到有inline出现,而且定义随处可见,就能将函数内联(上边已假设你的函数足够简单可以内联),而不必管是定义还是声明加inline的问题. 

所以,为了方便,将内联函数直接声明时就定义,放在头文件中.这样其它文件包含了该头文件,就在每个文件都出现了内联函数的定义.就可以内联了. 

类的成员函数也一样.只不过,类的成员函数缺省都是内联的,前提是你要在类定义时提供成员函数定义.如果在类定义时不提供函数定义,则要在类外边加上inline,否则将视为普通函数. 

关于这些,你看了C++primer有关函数那章自然会明白的. 

呵呵,你看的还是英文版的哦.你理解的不是这样,放在头文件中不是只存在一个定义,而是只要包含了该头文件的程序文本文件都存在了这个定义.而inline函数是可以被重复定义的,在C++中,常量对象跟内联函数都是可以多次定义的. 
你要把函数那章都看完就会明白. 

我先假设你的函数符合内联的条件. 

在声明是加inline,定义时不加,则要求编译器编译时,能看到inline的声明,而且在展开点看到该定义,这样,就将其视为内联函数. 

如果你声明没有inline,却在定义时inline了.这时,如果其它要调用该函数的文件看到了它的声明,就认为该函数不是内联的,所以,到了调用处, 转到该函数实现的地方,却意外地看到了inline声明,这时,会导致链接出错.若要改正的话,就要让调用该函数的文件也看到有inline的定义,而不 是在调用时才看到.你可以在每个文件都加上有inline的定义.(如果不加inline,则会出现重复定义的错误,因为内联函数才可以被重复定义).或 者另一种修改方法,你将定义时的inline去掉,这样就成为普通函数,链接不会出错.如果是前一种改法,仍是内联的,因为符合了看到了inline且随 处可见其定义的条件. 
如果你将声明跟定义都放在同一个头文件,而在声明时不内联,在实现时内联,这样编译器也是将该函数内联(符合两个条件,看到inline的声明(虽然是在定义时),随处可见其定义). 

总结说来,只要编译器看到有inline出现,而且定义随处可见,就能将函数内联(上边已假设你的函数足够简单可以内联),而不必管是定义还是声明加inline的问题. 

所以,为了方便,将内联函数直接声明时就定义,放在头文件中.这样其它文件包含了该头文件,就在每个文件都出现了内联函数的定义.就可以内联了. 

类的成员函数也一样.只不过,类的成员函数缺省都是内联的,前提是你要在类定义时提供成员函数定义.如果在类定义时不提供函数定义,则要在类外边加上inline,否则将视为普通函数. 

关于这些,你看了C++primer有关函数那章自然会明白的.

[原文](http://blog.csdn.net/baozi3026/article/details/5372268)