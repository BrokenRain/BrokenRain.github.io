---
title: Qt 杂录
date: 2019-06-04 11:51:54
tags:
 - Qt
---

# UUID
```
#include <QUuid>
QString uuid = QUuid::createUuid().toString().replace("{", "").replace("-", "").toUpper();
```

<!--more-->

# MD5
```
#include <QCryptographicHash>
QString md5 = QCryptographicHash::hash("Biao", QCryptographicHash::Md5).toHex();
```

# 随机数
Qt5.10后推荐使用 **QRandomGenerator** 生成随机数，而不再推荐qrand()：
```
#include <QRandomGenerator>
QRandomGenerator::global()->generate();   //(0, MAX_INT)
QRandomGenerator::global()->bounded(100); //(0, 100)
```