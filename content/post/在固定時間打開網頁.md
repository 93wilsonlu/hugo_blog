---
title: "在固定時間打開網頁"
date: 2020-07-29
draft: false
tags: ["Python"]
categories: ["Python"]
---

# 在固定時間打開網頁

原本打算每天晚上念一個小時英文，但是常常忘記，或是正在打比賽之類。

所以我想說如果能定時打開念英文的網頁，是不是就能提升念英文的頻率?

## 用 Python 打開網頁

```python
from webbrowser import open_new_tab

open_new_tab('https://quizlet.com')
```

用 pyinstaller 包成 exe

```cmd
pyinstaller.exe -F openWebsite.py
```

執行檔會在當前資料夾下 dist 資料夾裡

## 如何定時執行程式

按開始>Windows 系統管理工具>工作排程器

建立基本工作
![](/img/open-website-on-time/step1.png)
![](/img/open-website-on-time/step2.png)
![](/img/open-website-on-time/step3.png)
![](/img/open-website-on-time/step4.png)
![](/img/open-website-on-time/step5.png)

放 exe 的位置
![](/img/open-website-on-time/step6.png)

之後按完成就可以了
