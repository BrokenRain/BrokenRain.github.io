---
title: QT使用render时pixmap背景不为透明的解决办法
date: 2019-08-02 10:37:31
tags:
	- QT
---

当我们需要将一个界面绘制成图片时，就需要使用到render方法。
```
QPixmap pixmap(pwidget->size());
pwidget->render(&pixmap);
```

<!--more-->

如果pwidget背景为透明时，pixmap的背景并不是透明的，会自动填充一个背景框，这样就达不到我们想要的效果，而且好丑。
其实解决方法很简单，只需要将pixmap用透明色填充一下就能得到透明背景了。
```
QPixmap pixmap(pwidget->size());
pixmap.fill(QColor(0, 0, 0, 0));
pwidget->render(&pixmap);
```