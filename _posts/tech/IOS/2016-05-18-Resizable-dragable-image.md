---
layout: post
title: 使用ScrollView实现图片缩放、拖动
category: iOS
tags: iOS,图片
keywords: iOS,图片
description:
---

###	视图结构
---

##	UIScrollView 嵌套 UIImageView

![image](../../../upload/Snip20160518_3.png)


###	添加双击手势
---

```swift
- (void)awakeFromNib {
    [super awakeFromNib];
    UITapGestureRecognizer *doubleTap = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(doubleTap:)];
    doubleTap.numberOfTapsRequired = 2;
    doubleTap.numberOfTouchesRequired = 1;
    [self.previewImageView addGestureRecognizer:doubleTap];
    self.zoomEnabled = NO;//禁用缩放
}

- (void)doubleTap:(UIGestureRecognizer *)sender {
    
    CGPoint locationDep = [sender locationInView:sender.view];
    // CGSize(0~1,0~1)
    CGPoint zoomImgLocation = CGPointMake(locationDep.x / sender.view.bounds.size.width, locationDep.y / sender.view.bounds.size.height);

    if (self.zoomScroll.zoomScale < self.zoomScroll.maximumZoomScale) {
        
        CGFloat desScale = self.zoomScroll.maximumZoomScale;
        CGSize unZoomedImgSize = sender.view.bounds.size;
        
        CGSize zoomedSize;
        zoomedSize.width = unZoomedImgSize.width * desScale;
        zoomedSize.height = unZoomedImgSize.height * desScale;
        
        CGFloat maxOffsetX = (desScale - 1) * zoomedSize.width;
        CGFloat maxOffsetY = (desScale - 1) * zoomedSize.height;
        
        CGPoint offSet = CGPointMake(zoomedSize.width * zoomImgLocation.x, zoomedSize.height * zoomImgLocation.y);
        offSet.x = offSet.x - zoomedSize.width / desScale / 2;
        offSet.y = offSet.y - zoomedSize.height / desScale / 2;
        offSet.x = offSet.x > maxOffsetX ? maxOffsetX : offSet.x;
        offSet.y = offSet.y > maxOffsetY ? maxOffsetY : offSet.y;
        offSet.x = offSet.x < 0.0 ? 0.0 : offSet.x;
        offSet.y = offSet.y < 0.0 ? 0.0 : offSet.y;
        
        [UIView animateWithDuration:0.5 animations:^{
            self.zoomScroll.zoomScale = desScale;
            self.zoomScroll.contentOffset = offSet;
        }];
    }
    else {
        [self.zoomScroll setZoomScale:1.0 animated:YES];
        [self.zoomScroll setContentOffset:CGPointZero animated:YES];//放大双击区域
    }

}

- (void)setZoomEnabled:(BOOL)zoomEnabled {
    if (_zoomEnabled != zoomEnabled) {
        _zoomEnabled = zoomEnabled;
    }
    self.zoomScroll.userInteractionEnabled = zoomEnabled;
    self.previewImageView.userInteractionEnabled = zoomEnabled;
}
```

###	修改UIImageView尺寸适配图像
---

避免图像缩放时边缘空白

```swift
- (void)layoutSubviews {
    [super layoutSubviews];
    
    if (self.zoomScroll.zoomScale == 1) {

        CGSize imageS = self.previewImageView.image.size;
        CGSize cellSize = self.bounds.size;
        
        CGFloat expectedImageWidth = imageS.width / imageS.height * cellSize.height;
        CGFloat imageViewWidth = cellSize.width;
        
        BOOL fillWidth = imageViewWidth > expectedImageWidth;
        
        if (fillWidth) {
            self.imgEnding.constant = self.imgLeading.constant = (imageViewWidth - expectedImageWidth) / 2;
        }
        else {
            CGFloat expectedImageHeight = imageS.height / imageS.width * cellSize.width;
            CGFloat imageViewHeight = cellSize.height;
            self.imgTop.constant = self.imgBottom.constant = (imageViewHeight - expectedImageHeight) / 2;
        }
        
        [self updateConstraintsIfNeeded];
    }

}
```

###	添加UISCrollViewDelegate
---

PinchGesture动作缩放对象

```swift
-(UIView *)viewForZoomingInScrollView:(UIScrollView *)scrollView{
    return self.previewImageView;
}
```



