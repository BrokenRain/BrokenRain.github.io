---
title: Qt使用PNG时提示iCCP警告的解决方法
date: 2019-06-05 15:30:19
tags:
 - Qt
 - PNG
 - ICCP
---

使用Qt开发使用某些格式的png图片时，可能会报libpng warning: iCCP: known incorrect sRGB profile的警告，虽然并不影响程序的运行，但是看起来烦，而且影响调试信息的使用。

其实解决方法非常简单，只需要将png图片用QImage读取再保存就可以了。

这里推荐一个工具:[JQTools](https://github.com/188080501/JQTools/releases/latest)
[源码地址](https://github.com/188080501/JQTools)

# JQTools去除ICCP方法
* 选择**Qt相关**->**PNG警告消除**
* 选择图片就可轻松解决警告了