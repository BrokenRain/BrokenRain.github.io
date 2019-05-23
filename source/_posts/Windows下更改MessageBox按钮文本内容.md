---
title: Windows下更改MessageBox按钮文本内容
date: 2019-05-22
reward: true
tags:
  - Win32
---

在实际开发中，我们可能会碰到需要修改Windows系统下的MessageBox弹窗中按钮的文本内容，本文将介绍一种修改MessageBox弹窗中按钮内容的方法。
## 定义我们自己的弹窗方法MyMessageBox
```bash
LRESULT CALLBACK CBHookProc(int nCode, WPARAM wParam, LPARAM lParam)
{
	HWND hwnd = (HWND)wParam;
	if (nCode == HCBT_ACTIVATE)
	{
		SetDlgItemText(hwnd, IDYES, L"按钮内容");
		SetDlgItemText(hwnd, IDNO, L"按钮内容");
		...//可根据自己需求修改其他按钮
	}
	return 0;
}
int MyMessageBox(HWND hwnd, TCHAR *szText, TCHAR *szCaption, UINT uType)
{
	int ret;
	HHOOK hHook = SetWindowsHookEx(WH_CBT, CBHookProc, nullptr, GetCurrentThreadId());
	ret = MessageBoxEx(hwnd, szText, szCaption, uType, MAKELANGID(LANG_ENGLISH, SUBLANG_ENGLISH_US));
	UnhookWindowsHookEx(hHook);
	return ret;
}
```
## 使用MyMessageBox
```bash
MyMessageBox(MainFrameHwnd, _T("内容"), _T("标题"), MB_YESNO);
```
效果图
![](https://i.imgur.com/OnlLc6L.png)