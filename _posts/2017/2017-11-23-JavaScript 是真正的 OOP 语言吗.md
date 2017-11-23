---
layout: post
title: JavaScript 是真正的 OOP 语言吗
categories: [js]
description: JavaScript 是真正的 OOP 语言吗
keywords: js
---

JavaScript 是真正的 OOP 语言吗

## 序言

JavaScript面向对象还是不面向对象，这是个问题。好吧，这就是我们将要在这篇文章中讨论的主题。

我知道，这个话题已经被讨论过太多次了。但是，它总是被不断地提及。每当 Java 或 C# 或任何其他 OOP 语言的开发人员与 JavaScript 接触时，这些开发人员都会抱怨连连。他们说，用 JavaScript 工作简直是一团乱，没有类型，结构不合理，有些怪异，对象支持不给力，它绝对不是 OOP 语言。

其中有一些抱怨可能可以接受，但还有一些则是偏见，例如说 JavaScript 没有类型因而它不是 OOP 语言的言论。关于后面一点，在出口论断之前，你应该问自己：是什么使编程语言成为面向对象的编程语言？

## 什么是OOP？

OOP模式没有正式的标准规范。没有一个技术文档定义了什么是 OOP，什么不是 OOP。OOP 定义主要基于早期研究人员，如 Kristen Nygaard, Alan Kays, William Cook 等人发表的论文中的常识。已经有很多人尝试定义 OOP 以及一个可广泛接受的定义来对编程语言进行分类，因为面向对象基于两个要求：

- 通过对象建模问题的能力。
- 支持一些准许模块化和代码重用的原则。

为了满足第一个要求，这种语言必须使开发人员能够使用对象来描述现实并定义对象之间的关系，如下所示：

- 关联：对象引用另一个独立对象的能力。
- 聚合：对象嵌入一个或多个独立对象的能力。
- 组合：对象嵌入一个或多个依赖对象的能力。

通常，如果语言支持以下原则，则能满足第二个要求：

- 封装：专注于数据和操纵代码的单一实体，并隐藏其内部细节的能力。
- 继承：一个对象从一个或多个其他对象获取某些或所有要素的机制。
- 多态：根据数据类型或结构不同地处理对象的能力。

满足这些要求的语言我们通常将其归类为为面向对象的。

## JavaScript 和 OOP

所以现在我们知道 OOP 语言应该是什么样子的了。那么，我们可以证明 JavaScript 是一种 OOP 语言吗？咱们试试吧。

我们知道，JavaScript 对象支持关联，聚合和组合的能力并不强劲。请看以下代码：

```
var johnSmith = {
  firstName: "John",
  lastName: "Smith",
  address: { //Composition
    street: "123 Duncannon Street",  
    city: "London",
    country: "United Kingdom"
  }
};
var nickSmith = {
  firstName: "Nick",
  lastName: "Smith",
  address: { //Composition
    street: "321 Oxford Street",
    city: "London",
    country: "United Kingdom"
  }
};
johnSmith.parent = nickSmith; //Association
var company = {
  name: "ACME Inc.",
  employees: []
};
//Aggregation
company.employees.push(johnSmith);
company.employees.push(nickSmith);
```

在上面的代码中，你可以找到一个组合（address属性）的示例，一个关联（parent属性）的示例和一个聚合（employees属性）的示例。

至于封装，JavaScript 对象是支持数据和函数的实体，但它们没有高级的本地支持来隐藏内部细节。JavaScript 对象不关心隐私。如果不谨慎的话，所有的属性和方法都可以公开访问。但是，我们可以应用若干技术来定义对象的内部状态，并保护对象以防外部访问：使用 getter 和 setter 来利用[闭包](https://lhajh.github.io/js/2017/11/23/Javascript%E9%97%AD%E5%8C%85%E6%B7%B1%E5%85%A5%E8%A7%A3%E6%9E%90%E5%8F%8A%E5%AE%9E%E7%8E%B0%E6%96%B9%E6%B3%95.html)。

通过所谓的原型继承，JavaScript 在基本层中支持继承。即使有些开发人员认为它有点简单，但 JavaScript  的继承机制是完全有效的，并允许你得到与大多数公认的 OOP 语言相同的结果。任凭你怎么想，JavaScript 有一个机制，通过这个机制 `一个对象从一个或多个其他对象获取一些或所有的功能`，这就是继承。

有多态性的挑战似乎更加困难，因为许多人把这个概念与数据类型联系起来。实际上，多态性涉及编程语言的许多方面，并且不仅仅是与 OOP 语言有关。通常它涉及诸如泛型、重载和结构子类型等条目。所有这些对于一种“简单”和弱类型的语言—— JavaScript ——来说似乎不堪重负。然而事实并非如此：在 JavaScript 中，我们可以通过若干方式实现不同类型的多态，也许我们在不知不觉中已经做过很多次了。

## 没有类的 OOP

> 好吧，但话说回来，JavaScript 没有类。

许多开发人员认为 JavaScript 缺乏类的概念，而没有将 JavaScript 视为一种真正的面向对象的语言，因为它不强制符合 OOP 原则。

但是，我们可以看到，在非正式的定义中，并没有明确提及类。诚然，对象需要特性和原理。但类并非真正的要求，只是有时，类是一种抽象具有公共属性的对象集的简便方法而已。因此，即使一种语言的支持对象没有类，它也可以是面向对象的语言，例如 JavaScript。

此外，OOP 原则的目的旨在得到支持。为了在语言中进行编程，OOP 原则不应该是强制规定的。开发人员可以选择使用允许他创建面向对象代码的构造，也可以选择不使用。许多人批评 JavaScript 是因为开发人员可以编写违反 OOP 原则的代码。但这只是程序员的选择，而不是语言的限制。其他的编程语言也会发生这样的事情，如 C++。

所以，我们可以得出这样一个结论，缺乏抽象类并允许开发人员自由使用或不使用支持 OOP 原理的功能，并非认定 JavaScript 是 OOP 语言的真正障碍。