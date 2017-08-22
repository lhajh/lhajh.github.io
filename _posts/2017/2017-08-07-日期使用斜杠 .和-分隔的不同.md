---
layout: post
title: 日期使用/ .和-分隔的不同
categories: JS
description: 日期使用/ .和-分隔的不同
keywords: 日期, date
---

日期使用/ .和-分隔的不同

![](/assets/images/posts/js/Z1DXZZr.png)
- 使用`-`分隔：
	- 月和日都够两位（不够两位使用0补足）
		- 将new Date()中的时间根据时区转换为中国时间(由于中国位于东八区，所以会早8小时)
	- 月和日只要有一个不够两位
		- new Date()中的时间是格林威治标准时间 (GMT)

![](/assets/images/posts/js/8w571nm.png)
- 使用`/和.`分隔：
	- new Date()中的时间是格林威治标准时间 (GMT)