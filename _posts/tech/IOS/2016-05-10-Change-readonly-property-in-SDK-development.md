---
layout: post
title: SDK开发中修改只读属性
category: iOS
tags: iOS,SDK
keywords: iOS,SDK
description:
---


# 在SDK开发中有一些属性暴露给外界的是readonly属性，那在SDK内部是如何修改这些属性的呢？

## 例如：
暴露readonly属性的类SampleClass
```swift
@interface SampleClass : NSObject

@property (nonatomic, copy, readonly) NSString* aReadOnlyPropertyString;

@end
```

SampleClassB提供了属性(SampleClass *)sample
```swift
@interface SampleClassB : NSObject

@property (nonatomic, strong) SampleClass *sample;

@end
```

### SampleClassB中是如何对aReadOnlyPropertyString初始化的呢？
众所周知extension中可以修改属性的读写，于是我加了一个不暴露的头文件SampleClass_private.h
```swift
#import <SampleSDK/SampleClass.h>

@interface SampleClass ()

@property (nonatomic, copy, readwrite) NSString* aReadOnlyPropertyString;

@end

```

### 从而实现了只读属性的内部写操作
[测试项目地址]: https://github.com/leonyue/ProblemSolutions/tree/master/BuildSDK
