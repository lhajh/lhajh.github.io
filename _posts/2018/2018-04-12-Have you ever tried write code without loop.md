---
layout: post
title: 你曾经尝试过不用循环语句撸代码吗
categories: [JS]
description: 你曾经尝试过不用循环语句撸代码吗
keywords: js, no if
---

试着不用循环撸代码，是件很有趣的事，而且，万一你领会了什么是“数据即代码，代码即数据”呢？

这样做有什么意义吗？

事实上，它可以迫使你从不同的角度寻找解决方法，也许可以找到更好的方法。

当然，使用循环语句没有任何不对的地方。但是，不使用循环的话，有时候可以增加代码的可读性。这一点并不是绝对的，如果完全不使用循环语句的话，代码可读性也许会更差。这需要你根据不同情况去判断。

而且，不用循环的话不只是影响可读性。在这背后隐含着更加深刻的道理。通过了解本文的代码示例，你可以发现，如果不使用循环语句的话，你的代码会更加接近代码即数据的概念。

另外，当你尝试不使用循环语句去编程时，也是一件非常有意思的事情。

## 示例1: 统计数组中的奇数

假设我们有一个整数数组 arrayOfIntegers，现在需要统计其中奇数的个数：
```
const arrayOfIntegers = [1, 4, 5, 9, 0, -1, 5];
```
使用 if
```
let counter = 0;
arrayOfIntegers.forEach(integer => {
  const remainder = Math.abs(integer % 2);
  if(remainder === 1) {
    counter++;
  }
});

console.log(counter);
```
不用if
```
let counter = 0;
arrayOfIntegers.forEach(integer => {
  const remainder = Math.abs(integer % 2);
  counter += remainder;
});

console.log(counter);
```
不用 if 时，我们巧妙地利用了奇数与偶数的特性，它们除以 2 的余数分别是 0 和 1。

## 示例2： 判断工作日和周末

给定一个日期(比如 new Date())，判断它是工作日还是周末，分别返回”weekend”和”weekday”。

使用if
```
const weekendOrWeekday = inputDate => {
  const day = inputDate.getDay();
  if(day === 0 || day === 6) {
    return'weekend';
  }
  return'weekday';

// Or, for ternary fans:
// return (day === 0 || day === 6) ? 'weekend' : 'weekday';
};

console.log(weekendOrWeekday(newDate()));
```
不用if
```
const weekendOrWeekday = inputDate => {
  const day = inputDate.getDay();
  return weekendOrWeekday.labels[day] || weekendOrWeekday.labels['default'];
};
weekendOrWeekday.labels = {
  0: 'weekend',
  6: 'weekend',
  default: 'weekday'
};

console.log(weekendOrWeekday(new Date()));
```
你是否发现 if 语句中其实隐含着一些信息呢？它告诉我们哪一天是周末，哪一天是工作日。因此，要去掉 if 语句的话，我们只需要把这些信息写入 weekendOrWeekday.labels 对象，然后直接使用它就好了。

## 示例3: doubler 函数

写一个 doubler 函数，它会根据参数的类型，进行不同的操作：

- 如果参数是数字，则乘以 2(i.e. 5 => 10, -10 => -20)；
- 如果参数是字符串，则每个字符重复 2 次 (i.e. 'hello' => 'hheelloo')；
- 如果参数是函数，则调用 2 次；
- 如果参数是数组，则将每一个元素作为参数，调用 doubler 函数
- 如果参数是对象，则将每个属性值作为参数，调用 doubler 函数

使用 switch
```
const doubler = input => {
  switch(typeof input) {
    case 'number':
      return input + input;
    case 'string':
      return input.split('').map(letter => letter + letter).join('');
    case 'object':
      Object.keys(input).map(key => (input[key] = doubler(input[key])));
      return input;
    case 'function':
      input();
      input();
  }
};

console.log(doubler(-10));
console.log(doubler('hey'));
console.log(doubler([5, 'hello']));
console.log(doubler({ a: 5, b: 'hello'}));
console.log(doubler(function() {
  console.log('call-me');
}));
```
不用switch
```
const doubler = input => {
  return doubler.operationsByType[typeof input](input);
};

doubler.operationsByType = {
  number: input => input + input,
  string: input => input.split('').map(letter => letter + letter).join(''),
  function: input => {
    input();
    input();
  },
  object: input => {
    Object.keys(input).map(key => (input[key] = doubler(input[key])));
    return input;
  }
};

console.log(doubler(-10));
console.log(doubler('hey'));
console.log(doubler([5, 'hello']));
console.log(doubler({ a: 5, b: 'hello'}));
console.log(doubler(function() {
  console.log('call-me');
}));
```
可知，我将每一种参数类型对应的操作绑定到了 doubler.operationsByType，这样不需要 switch 语句，就可以实现 doubler 函数了。

## 示例4: 数组元素求和

数组如下:
```
const arrayOfNumbers = [17, -4, 3.2, 8.9, -1.3, 0, Math.PI];
```
使用循环语句
```
let sum = 0;

arrayOfNumbers.forEach(number => {
  sum += number;
});

console.log(sum);
```
可知，我们需要通过修改 sum 变量，来计算结果。

不用循环语句

使用 reduce 方法时，就可以避免使用循环语句了：
```
const sum = arrayOfNumbers.reduce((acc, number) =>
  acc + number
);

console.log(sum);
```
实现递归，同样可以避免使用循环语句：
```
const sum = ([number, ...rest]) => {
  if (rest.length === 0) { 
    return number || 0;
  }
  return number + sum(rest);
};

console.log(sum(arrayOfNumbers))
```
可知，代码中巧妙地使用了一个ES6语法 - 扩展运算符。rest 代表了除去第一个元素之后的剩余数组，它的元素个数会随着一层层递归而减小直至为 0，这样就满足了递归结束的条件。这种写法非常机智，但是在我看来，可读性并没有使用 reduce 方法那么好。

## 示例5: 将数组中的字符串拼接成句子

数组如下，我们需要过滤掉非字符串元素：
```
const dataArray = [0, 'H', {}, 'e', Math.PI, 'l', 'l', 2/9, 'o!'];
```
目标结果是“Hello!”.

使用循环语句
```
let string = '', i = 0;

while (dataArray[i] !== undefined) {
  if (typeof dataArray[i] === 'string') {
    string += dataArray[i];
  }
  i += 1;
}

console.log(string);
```
不用循环语句

使用 filter 和 join 方法的话，可以避免使用循环语句：
```
const string = dataArray.filter(e => typeof e === 'string').join('');

console.log(string);
```
可知，使用 filter 方法还帮助我们省掉了 if 语句。

## 示例6: 将数组元素变换为对象

数组元素为一些书名，需要将它们转换为对象，并为每一个对象添加 ID：
```
const booksArray = [
  'Clean Code',
  'Code Complete',
  'Introduction to Algorithms',
];
```
目标结果如下：
```
newArray = [
  { id: 1, title: 'Clean Code' },
  { id: 2, title: 'Code Complete' },
  { id: 3, title: 'Introduction to Algorithms' },
];
```
使用循环语句
```
const newArray = [];
let counter = 1;

for (let title of booksArray) {
  newArray.push({
    id: counter,
    title,
  });

  counter += 1;
}

console.log(newArray);
```
不用循环语句

使用 map 方法的话，可以避免使用循环语句：
```
const newArray = booksArray.map((title, index) => ({ 
  id: index + 1, 
  title 
}));

console.log(newArray);
```

一张图片解释 map、filter 和 reduce 的作用

![](/assets/images/posts/js/loop.jpg)

## 参考资料

- [你试过不用if撸代码吗？](https://blog.fundebug.com/2017/11/06/write-javascript-without-if/)
- [试过不用循环语句撸代码吗](https://blog.fundebug.com/2017/11/13/write-javascript-without-loop/)