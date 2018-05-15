---
layout: post
title: JavaScript 时间处理之几个月前或几个月后的指定日期
categories: [JS]
description: JavaScript 时间处理之几个月前或几个月后的指定日期
keywords: Date
---

在平常项目开发过程中,经常会遇到需要在 JavaScript 中处理时间的情况，无非两种（1，逻辑处理  2，格式转换处理）。当然要说相关技术博，网上闭着眼睛都能抓一把，但是我要做的是：既然有幸被我碰到了，就要尽可能的分析转化为最适合自己的东西，成为自己知识库的一部分；同时希望能帮助有需要的同学解决遇到的相关小问题。

此类型常用需求为：推算几个月后（前）的今天的日期

```js
/**
 * 根据指定日期往前或往后推多少月
 * @param  {Number || String} num [往前或往后推多少月]
 * @param  {Boolean} isFirstDay [日是否是第一天，默认跟随时间推]
 * @param  {Date} date [日期，默认今天]
 * @return {Object}    [指定年月的开始时间和结束时间]
 */
const scheduledMonth = (num, isFirstDay, date) => {
  // 参数只有 2 个：推几个月和指定日期
  if (isFirstDay && isFirstDay.constructor !== Boolean && date === undefined) {
    date = isFirstDay
    isFirstDay = false
  }
  if (date) {
    if (typeof date === 'number') {
      date = new Date(date)
    } else if (typeof date === 'string') {
      if (isNaN(new Date(date).getTime())) {
        console.error('Invalid Date')
        return
      } else {
        date = new Date(date)
      }
    }
  }
  if (typeof num === 'number') {
    return previous(num, isFirstDay, date)
  } else if (typeof num === 'string') {
    num = +num
    if (isNaN(num)) {
      console.error('Invalid Number')
      return
    } else {
      return previous(num, isFirstDay, date)
    }
  } else {
    console.error('Invalid Number')
    return
  }
}
const previous = (num, isFirstDay, date) => {
  date = date || new Date()
  let year = date.getFullYear()
  let month = date.getMonth() + 1
  let day = date.getDate()
  let quotient = parseInt(num / 12)
  let remainder = num % 12
  let previousYear = year + quotient
  let previousMonth = month + remainder
  let previousDay = day
  if (previousMonth > 12) {
    previousYear++
    previousMonth -= 12
  } else if (previousMonth <= 0) {
    previousYear--
    previousMonth += 12
  }
  if (isFirstDay) {
    previousDay = day = 1
  } else {
    // 获取当前日期中月的天数
    let lastDate = new Date(previousYear, previousMonth, 0).getDate()
    previousDay = lastDate < previousDay ? lastDate : previousDay
    // 这里巧妙使用当月总天数来避开下面月份不足 31 天的处理
    // if (day >= 29) {
    //   if (previousMonth === 2) {
    //     if (isLeapYear(previousYear)) {
    //       previousDay = 29
    //     } else {
    //       previousDay = 28
    //     }
    //   }
    // }
    // if (day === 31) {
    //   if (/[4,6,9,11]/.test(previousMonth)) {
    //     previousDay = 30
    //   }
    // }
  }
  day = ltTen(day)
  previousDay = ltTen(previousDay)
  month = ltTen(month)
  previousMonth = ltTen(previousMonth)
  if (num < 0) {
    return {
      fromTime: `${previousYear}${previousMonth}${previousDay}`,
      toTime: `${year}${month}${day}`
    }
  } else {
    return {
      fromTime: `${year}${month}${day}`,
      toTime: `${previousYear}${previousMonth}${previousDay}`
    }
  }
}
// 满足闰年的条件：四年一闰，百年不闰，四百年再闰
// 条件 1：年份必须要能被4整除
// 条件 2：年份不能是整百数
// 条件 3：年份是400的倍数
// 当条件 1 和条件 2 同时成立时，就肯定是闰年，所以条件 1 和条件 2 之间为“与”的关系。
// 如果条件 1 和条件 2 不能同时成立，但如果条件 3 能成立，则仍然是闰年。所以条件 3 与前 2 项为“或”的关系。
const isLeapYear = (year) => {
  return year % 4 === 0 && year % 100 !== 0 || year % 400 === 0
}
const ltTen = (num) => {
  return num < 10 ? '0' + num : num
}
```

说明一下：自己封装好的是通过是否是闰年来判断 2 月的，感谢[@MonkeyChen.K.](http://www.cnblogs.com/peixuanzhihou/p/6204551.html)提供的 获取当前日期中月的天数 这个巧妙方法

关于 `new Date()` 传不同参数返回不同结果，可以看看这篇文章：[JavaScript Date 对象与函数](http://www.dreamdu.com/javascript/object_date/)