---
title: Javascript中的原型链
date: 2017-07-31 10:28:54
tags:
	- javascript
	- 原型链
	-	对象
categories: javascript
---

## 万物皆对象？

对于一个没有其他语言基础的人来说，js中的对象是比较让人困惑的，同时也是js中最难以掌握和使用的部分。我觉得要想搞清楚js中的原型链，首先得要搞清楚js中的对象。

其实我经常在各种js教程中看到这么一句话：“万物皆对象”。当时作为初学者的我并不能理解这句话的意思，因为我只知道js中的数据类型分为基本类型和引用类型，其中引用类型又包含Object、Function、Date、RegExp和Array。他们都继承自金字塔顶端的男人：Object.prototype,把他们称作对象，我可以理解。但是那些基本数据类型和对象有什么关系吗？后来，在我翻阅《高程》时，知道了原来有基本包装类型这回事。当我们读取基本数据类型时，后台会创建一个对应的基本包装类型的对象，所以我们才能调用一些对应的方法来操作数据，比如String类型的substring。当然，这其中的操作没有摆在明面上，是后台自动完成了这些操作。至此，我才明白了为什么说js中“万物皆对象”。

## Javascript中的原型链

其实还有一句我看到过无数次的话：“javascript不同于其它面向对象语言，它没有类的概念，通过原型链的方式来实现继承”。当时看到“面向对象”、“类”、“原型链”这些“高大上”的词语时，本小白的内心是有些抵触的。到后来我在查看别人的源码时总是看到prototype、`__proto__`，这些让我莫名其妙的东西，我觉得是时候搞清楚了。于是，在我断断续续(没错，就是懒)、反反复复把《高程》第六章翻了好几遍，外加阅读了各种相关文章后。终于，我算是理顺了javascript中的原型链。所以，今天抽了个时间来和大家一起分享一下。

<!-- more -->

我们在js中创建的每一个函数都有prototype属性，它是一个指针，指向函数的原型对象，这个对象包含一些属性和方法，其中constructor属性指向函数本身。这些属性和方法会被当前函数生成的所有实例（对象）所共享。同时，生成的实例内部会自动生成一个指针，指向构造函数的原型对象，你可以通过`__proto__`属性或者getPrototypeOf()方法来获取指针的指向。最终，`obj.__proto__.__proto__`层层连接，直到Object.prototype结束，组成了js中的原型链。

其实，在我学习的过程中，有一个问题曾一度困扰我许久：

```
Function instanceOf Object; //true

Object instanceOf Function; //true
```

当时我以为原型链顶端的Object和构造函数Object不是同一个Object(思路清奇），发现这个问题后，我就想，在进行instanceOf操作的时候，编译器是如何知道Object是哪一个Object嘞？想来想去也没想出个头绪。后来看了几篇文章之后才明白。原来站在继承链顶端的并不是Object，而是Object.prototype，而Object是一个构造函数，继承于Function.prototype，即`Object.__proto__ === Function.prototype`而Function就是函数本身，即`Function.__proto__ === Function.prototype`。同时`Function.prototype.__proto__ === Object.prototype`。如下图所示：

![](https://camo.githubusercontent.com/b8806bd76878881e7f843b0e77643aff26594ecf/687474703a2f2f3773626e62612e636f6d312e7a302e676c622e636c6f7564646e2e636f6d2f6769746875622d6a732d70726f746f747970652e6a7067)

至此，问题就解决了。从图中我们还可以看出，javascript中的所有构造函数都继承自Function.prototype。每当我们获取一个实例的属性或者方法时，就会从实例内部开始，沿着这条原型链一步一步向上找，直到找到或者返回undefined。

## 结语

了解js的原型链可以帮助我以后更好的了解继承方面的知识，并且在js的面向对象编程中也有着巨大的作用。希望我能早日将所学运用到实际项目中，写出简洁、优雅、封装良好的js代码。