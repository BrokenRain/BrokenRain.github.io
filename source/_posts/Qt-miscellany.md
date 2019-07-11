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

# connect使用有重载的信号或槽函数
## 方法一: 使用`static_cast<>()`进行强制类型转换

以QSpinBox为例:
QSpinBox有一个重载信号void valueChanged(int i)和void valueChanged(const QString &text)。

```
使用connect(mySpinBox, &QSpinBox::valueChanged, mySlider, &QSlider::setValue)会编译报错，
因为编译器不知道该使用哪一个valueChanged信号。
```

正确使用
```
connect(mySpinBox, SIGNAL(valueChanged(int)), mySlider, SLOT(setValue(int));
或
connect(mySpinBox, static_cast<void (QSpinBox::*)(int)>(&QSpinBox::valueChanged), mySlider, &QSlider::setValue);
```

## 方法二
![](Qt-miscellany/connect使用有重载的信号或槽函数方法二.png)

```
使用QOverload<>::of()或者qOverload<>()(需要C++14支持)
例如：connect(mySpinBox,QOverload<int>::of(&QSpinBox::valueChanged), mySlider,&QSlider::setValue);
```

# 去除QLineEdit、QTextEdit等右键菜单
```
setContextMenuPolicy(Qt::NoContextMenu);
```

# 防止QSpinBox自动突出显示内容
```
connect(spinBox, SIGNAL(valueChanged(int)), this, SLOT(onSpinBoxValueChanged()), Qt::QueuedConnection);

void Window::onSpinBoxValueChanged() // slot
{
    spinBox->findChild<QLineEdit*>()->deselect();
}

```

# 删除布局中所有控件
```
void Tool::cleanLayout(QLayout *layout)
{
	while (0 != layout->layout()->count())
	{
		QWidget *widget = layout->layout()->itemAt(0)->widget();
		layout->layout()->removeWidget(widget);
		widget->setParent(nullptr);
		widget->deleteLater();
	}
}
```

# 删除QListWidget的Item
```
void Tool::clearListWidget(QListWidget *listWidget)
{
	while (0 != listWidget->count())
	{
		QListWidgetItem *item = listWidget->item(0);
		QWidget *widget = listWidget->itemWidget(item);
		if (nullptr != widget)
		{
			listWidget->removeItemWidget(item);
			widget->deleteLater();
		}

		item = listWidget->takeItem(0);
		delete item;
		item = nullptr;
	}
}
```