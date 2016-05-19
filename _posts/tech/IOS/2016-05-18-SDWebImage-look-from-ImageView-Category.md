---
layout: post
title: 乱入SDWebImage
category: iOS
tags: iOS,缓存,SDWebImage
keywords: iOS,缓存,SDWebImage
description:
---


###	小技巧
---

objc动态添加读取属性
	
```swift
static char imageURLKey;

objc_setAssociatedObject(self, &imageURLKey, url, OBJC_ASSOCIATION_RETAIN_NONATOMIC);

return objc_getAssociatedObject(self, &imageURLKey);
```

NS_OPTIONS判断

```swift
if (!(options & SDWebImageDelayPlaceholder))
```

安全主线程

```swift
#define dispatch_main_sync_safe(block)\
    if ([NSThread isMainThread]) {\
        block();\
    } else {\
        dispatch_sync(dispatch_get_main_queue(), block);\
    }

#define dispatch_main_async_safe(block)\
    if ([NSThread isMainThread]) {\
        block();\
    } else {\
        dispatch_async(dispatch_get_main_queue(), block);\
    }
```

检查是否遵照协议

```swift
([operations conformsToProtocol:@protocol(SDWebImageOperation)])
```

@synchronized锁

```swift
    @synchronized (self.failedURLs) {
        isFailedUrl = [self.failedURLs containsObject:url];
    }
```

内联函数

[static inline 和 extern inline](http://leonyue.github.io/2016/05/19/static-inline-extern-inline.html)

```swift
FOUNDATION_STATIC_INLINE NSUInteger SDCacheCostForImage(UIImage *image) {
    return image.size.height * image.size.width * image.scale * image.scale;
}
```

MD5 计算

[参考](http://jingyan.baidu.com/article/09ea3ededca4b1c0aede39c4.html)

```swift
- (NSString *)cachedFileNameForKey:(NSString *)key {
    const char *str = [key UTF8String];
    if (str == NULL) {
        str = "";
    }
    unsigned char r[CC_MD5_DIGEST_LENGTH];
    CC_MD5(str, (CC_LONG)strlen(str), r);
    NSString *filename = [NSString stringWithFormat:@"%02x%02x%02x%02x%02x%02x%02x%02x%02x%02x%02x%02x%02x%02x%02x%02x%@",
                          r[0], r[1], r[2], r[3], r[4], r[5], r[6], r[7], r[8], r[9], r[10],
                          r[11], r[12], r[13], r[14], r[15], [[key pathExtension] isEqualToString:@""] ? @"" : [NSString stringWithFormat:@".%@", [key pathExtension]]];

    return filename;
}
```