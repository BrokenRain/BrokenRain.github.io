---
title: Qt pro使用记录
date: 2019-09-09 17:41:47
tags:
	- QT
---

## 平台、编译器位数区分

<!--more-->

```
win32 {
    contains(QT_ARCH, i386) {
    message("32-bit")
    LIBS += ...... (32位库)
    }else {
    message("64-bit")
    LIBS += ...... (64位库)
    }
}

macx {
    
}

unix {

}

```