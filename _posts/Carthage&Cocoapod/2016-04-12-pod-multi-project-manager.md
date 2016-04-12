---
layout: post
title: 使用Pods和自定义静态库实现多工程联编
category: 三方库管理
tags: Cocoapods
keywords: Cocoapods,pods
description:
---



近来随着公司项目开发的深入，项目的规范也就越来越高了，为了更加方便的管理自定义静态库与pods之间的联系，好好的研究了一番。

##  1.首先就是创建一个静态库，命名为Mark（这个随便你自己怎么定义）。
---
![image](http://127.0.0.1:4000/upload/80230176-7A80-46A5-B4CF-9EFE6D6B9A98.png)

##  2.创建一个实例工程，（也就是你需要开发的工程 或者是你已经创建好了的）。
---

##  3.书写pod file文件。（非常关键，因为格式有时候会造成很多的坑）
---

workspace ‘MarkSpace’//需要生成workspace的名称

xcodeproj ‘Mark/Mark.xcodeproj'//静态库的名称

xcodeproj ‘MarkDemo/MarkDemo.xcodeproj'//实例工程名称

target :Mark do//静态库需要引入的三方库

platform :ios, '7.0'

pod 'AFNetworking'

xcodeproj ‘Mark/Mark.xcodeproj'

end

target :MarkDemo do//实例工程需要引入的三方库

platform :ios, '7.0'

pod 'AFNetworking'

xcodeproj ‘MarkDemo/MarkDemo.xcodeproj'

end

##  4.执行pod install。（确保你的podfile文件，静态工程文件，和你的demo在同一工程目录下）
---

执行成功之后我的目录是这样的。
![image](http://leonyue.github.io/upload_images/596199-fd6388ca1ff93c71.png)

##  5.将自定义的静态库引入到主工程目录中去。
---

首先在Build Phases/Link Binary with Libraries中自定义的静态库添加进来。添加完成后我的是这样的。
![image](http://leonyue.github.io/upload_images/596199-c7f9952d11c0e973.png)
下一步就是将静态库的目录引用进来，在主工程的Target/Build Settings /User Header SearchPaths中添加$(BUILT_PRODUCTS_DIR),并且选择递归引用 也就是（recursive）。
![image](http://leonyue.github.io/upload_images/596199-a33542e1ed4e3762.png)

##  结语：
---

现在多工程联编已经是企业级应用的必备了，不断可以灵活的应用自己的静态库，也能随时的更新pods提供的三方库，让项目管理起来非常的方便。

##  附：pod install 加载慢的问题
---

最近使用CocoaPods来添加第三方类库，无论是执行pod install还是pod update都卡在了Analyzing dependencies不动

原因在于当执行以上两个命令的时候会升级CocoaPods的spec仓库，加一个参数可以省略这一步，然后速度就会提升不少。加参数的命令如下：

pod install --verbose --no-repo-update

pod update --verbose --no-repo-update



出现[!] Unable to find a pod with name matching `Af' 错误解决方案

[link](http://stackoverflow.com/questions/21342574/cocoapods-error-to-install-search-pods)

文／奔跑的码农（简书作者）
原文链接：http://www.jianshu.com/p/4cb36b7ccc99
著作权归作者所有，转载请联系作者获得授权，并标注“简书作者”。
