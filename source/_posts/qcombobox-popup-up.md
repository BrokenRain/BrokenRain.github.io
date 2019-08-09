---
title: QComboBox向上弹出
date: 2019-08-09 15:15:45
tags:
	- QT
---

QComboBox的popup默认是向下弹出的，但是当我们想要popup向上弹出时就很无奈了，也没有API给你设置，这种感觉就像你今天发工资，想去小巷子里资助那些贫困小姐姐时，但是你找不到小巷子在哪，你说气不气人。没办法，就只能自己解决了。

<!--more-->

## 重写showPopup函数
```
void MyQComboBox::showPopup() override
{ 
    QComboBox::showPopup(); 
    QWidget *popup = this->findChild<QFrame*>(); 
    popup->move(popup->x(),popup->y()-this->height()-popup->height()); 
}
```

但是发现效果并不理想，它会有个向下的动画，然后再向上弹出。虽然没资助成功，但体验不能差不是。

## 关闭弹窗的动画
```
class MyComboBox : public QComboBox
{
public:
	MyComboBox(QWidget *parent = nullptr) : QComboBox(parent)
	{
		this->view()->installEventFilter(this);
	}
	
protected:
	void showPopup() override
	{
		bool oldAnimationEffects = qApp->isEffectEnabled(Qt::UI_AnimateCombo);
		qApp->setEffectEnabled(Qt::UI_AnimateCombo, false);

		QComboBox::showPopup();
		qApp->setEffectEnabled(Qt::UI_AnimateCombo, oldAnimationEffects);
	}

	bool eventFilter(QObject *obj, QEvent *e) override
	{
		bool handled = false;
		if (e->type() == QEvent::Show)
		{
			if (obj == view())
			{
				QWidget *frame = findChild<QFrame*>();
				frame->move(frame->x(), frame->y() - this->height() - frame->height());
			}
		}

		if (!handled)
			handled = QComboBox::eventFilter(obj, e);

		return handled;
	}
};
```
这样就能实现向上弹出的QComboBox了，但是弹出时没有动画，显得有点僵硬，那么有没有其他更好的方案呢？这个就要靠大家一起思考了。小巷子再深，我相信大家还是有办法能把它找出来的。