---
layout: post
title: Location 对象
categories: [JS]
description: Location 对象
keywords: JavaScript, Location
---

Location 对象包含有关当前 URL 的信息。

Location 对象是 Window 对象的一个部分，可通过 window.location 属性来访问。

## Location 对象属性

|属性|	描述| 例子 | 返回值
|--|--|--|--|
|href	 |设置或返回完整的 URL。| `http://example.com:1234/test.htm#part2` | `http://example.com:1234/test.htm#part2`
|host	 |设置或返回主机名和当前 URL 的端口号。相当于 hostname + port，当端口为 80 时，和 hostname 一样| `http://example.com:1234/test.htm#part2` |  `example.com:1234` 
|hostname	 |设置或返回当前 URL 的主机名。| `http://example.com:1234/test.htm#part2` | `example.com`
|port	 |设置或返回当前 URL 的端口号。| `http://example.com:1234/test.htm#part2` | `1234` |
|protocol	 |设置或返回当前 URL 的协议。| `http://example.com:1234/test.htm#part2` | `http:`
|pathname	 |设置或返回当前 URL 的路径部分。| `http://example.com:1234/test.htm#part2` | `/test.htm`
|hash	 |设置或返回从井号 (#) 开始的 URL（锚）。| `http://example.com:1234/test.htm#part2` | `#part2`
|search	 |设置或返回从问号 (?) 开始的 URL（查询部分）。| `http://example.com:1234/test/t.asp?f=hdom_loc_search` | `?f=hdom_loc_search` |

**当一个 Location 对象被转换成字符串，href 属性的值被返回。这意味着你可以使用表达式 location 来替代 location.href。**

## Location 对象方法

|属性|	描述|
|--|--|
|assign()	|加载新的文档。|
|reload()	|重新加载当前文档。|
|replace()	|用新的文档替换当前文档。|

