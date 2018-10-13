---
title: Android分页组件Paging简单使用
date: 2018-10-10 17:03:23
tags: Paging
---

# Paging库引入：
`
implementation 'android.arch.paging:runtime:1.0.0'
`
刚引入就遇到了错误，真是一个不友好的开始:
```
Error:Execution failed for task ':app:transformDexArchiveWithExternalLibsDexMergerForDebug'.
> java.lang.RuntimeException: java.lang.RuntimeException: com.android.builder.dexing.DexArchiveMergerException: Unable to merge dex
```
一查资料，是导入重复的包，这个出错的地方还是google自己的东西，这岂不是
“大水冲了龙王庙，自家人打自家人”？
`
compile 'com.android.support:design:26.1.0'

`
google了一下，在stackoverflow中找到了解决办法：
https://stackoverflow.com/questions/49028119/multiple-dex-files-define-landroid-support-design-widget-coordinatorlayoutlayou
即用编译版本改成27，并且把对应的库版本也改为27
`
implementation 'com.android.support:appcompat-v7:27.1.1'
`
`
compile 'com.android.support:design:27.1.1'
`