---
layout: post
title: iOS视频编辑学习
category: iOS
tags: iOS
keywords: iOS
description:
---

模仿系统视频剪辑做的一个界面

![image](/assets/uploads/Untitled.gif)

###	总结
---

1、	CMTime的使用

自带一系列的计算方法，如：

减法

`CMTimeSubtract(self.endC, self.beginC)`

比较

`if (CMTimeCompare(cTime, self.endC) >= 0)`

除法

`CMTimeMultiplyByRatio(t, (int32_t)index, (int32_t)total);`

2、	AVPlayer seekToTime的坑

seekToTime有很大误差

`[self.player seekToTime:self.beginC toleranceBefore:kCMTimeZero toleranceAfter:kCMTimeZero];`

3、	判断AVPlayer的播放状态

可以通过rate属性,rate还可以控制播放速度

```swift
/* indicates the current rate of playback; 0.0 means "stopped", 1.0 means "play at the natural rate of the current item" */
@property (nonatomic) float rate;
```

`[object addObserver:self forKeyPath:@"rate" options:NSKeyValueObservingOptionInitial | NSKeyValueObservingOptionNew context:nil];`

4、	添加字幕时的问题

离线模式可以通过设置AVVideoCompositionCoreAnimationTool添加字幕,但是此方式对AVPlayerItem添加字幕失败了。参考如下,还没找到替代方案

```swift
/*!
    @class			AVVideoCompositionCoreAnimationTool

    @abstract		A tool for using Core Animation in a video composition.

 @discussion
   Instances of AVVideoCompositionCoreAnimationTool are for use with offline rendering (AVAssetExportSession and AVAssetReader), not with AVPlayer.
   To synchronize real-time playback with other CoreAnimation layers, use AVSynchronizedLayer.

   Any animations will be interpreted on the video's timeline, not real-time, so
		(a) set animation beginTimes to small positive value such as AVCoreAnimationBeginTimeAtZero rather than 0,
		    because CoreAnimation will replace a value of 0 with CACurrentMediaTime();
		(b) set removedOnCompletion to NO on animations so they are not automatically removed;
		(c) do not use layers associated with UIViews.
*/
```
