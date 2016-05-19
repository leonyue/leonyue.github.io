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