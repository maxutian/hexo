---
title: Javascript中的继承方法(1)
date: 2017-08-03 16:47:01
tags:
	- javascript
	- 继承
	- 原型
	- 技术
categories: javascript
---

{% note primary %}

本文章内容参考《Javascript高级程序设计》

{% endnote %}

前两天结合《高程》和网上的文章把js中的原型链梳理了一遍，终于算是把原型链弄清楚了。同时对js中“万物皆对象”的理解更深了一层。当然，只是单独理解这些对以后的编程帮助可能不是那么的大，理解这些主要还是为实现继承做铺垫。所以，让我们开始吧！

继承可以说是js中最重要的东西了，如果你想要编写出可复用并且封装良好的代码，面向对象和继承必不可少。当然，对于初学者来说要想完全理解和实现并说出各种继承方法的优劣是很不容易的，更不用说在实际编程中熟练的运用了。所以今天我们先从js继承方法中比较简单的3种入手。

<!-- more -->

# 基于原型链的继承

## 基本思想

上一篇文章中提到的原型链在实际工作中最大的作用就是实现js中对象的继承。我们知道，当一个函数被创建时，同时还会创建一个prototype对象，在我们调用构造函数创建一个实例对象时，实例对象中的\[[prototype]]属性会指向构造函数的原型对象，当我们调用实例对象中的属性或者方法，解析引擎会沿着实例对象——>实例对象的\__proto\__对象——>实例对象的\__proto\__对象的\__proto\__对象寻找，直到找到。这就构成了最简单的继承。

## 实现

```
function SuperType(){
	this.property = true;
}

SuperType.prototype.getSuperValue = function(){
	return this.property;
};

function SubType(){
	this.subproperty = false;
}

SubType.prototype = new SuperType();

SubType.prototype.getSubValue = function (){
	return this.subproperty;
};

var instance = new SubType();
alert(instance.getSuperValue());
```

这里，将子类Subtype的prototype对象重写为超类SuperType的一个实例，instance——>SubType.prototype——>SuperType.prototype就构成了简单的继承。记住，当你使用原型链方式实现继承时，要谨慎使用字面量方法重写原型对象，因为这样会导致新的原型对象的\[[prototype]]属性指向Object.prototype，从而导致原型链的断裂，继承的失效。

## 局限性

### 原型对象中包含引用类型的值

```
function SuperType(){
	this.clolors = ['red', 'blue', 'green'];
}

SuperType.prototype.getSuperValue = function(){
	return this.property;
};

function SubType(){
}

SubType.prototype = new SuperType();

var instance1 = new SubType();
instance1.colors.push('black');
alert(instance1.colors); //'red', 'blue', 'green', 'black'

var instance2 = new SubType();
alert(instance2.colors); //'red', 'blue', 'green', 'black'
```

当原型对象中包含引用类型的值时，只要有一个实例对象中对这个值进行操作，结果将在所有实例对象中反映出来。

### 不能传参

这种方法的第二个问题是，在创建子类型(SubType)的实例时，不能向超类型(SuperType)的构造函数中传递参数。实际上，应该说是没有办法在不影响所有对象实例的情况下，给超类型的构造函数传递参数。

# 借用构造函数

## 基本思想

这种技术有时也叫做伪造对象或者经典继承。这种技术的基本思想相当简单，即在子类型构造函数内部调用超类型的构造函数。由于构造函数也是函数，所以在子类中通过call方法绑定超类构造函数的作用域为子类的作用域，就实现了子类构造函数的初始化，并且所有属性和方法都来自于超类。

## 实现

```
function SuperType(){
	this.clolors = ['red', 'blue', 'green'];
}

function SubType(){
	SuperType.call(this);
}

var instance1 = new SubType();
instance1.colors.push('black');
alert(instance1.colors); //'red', 'blue', 'green', 'black'

var instance2 = new SubType();
alert(instance2.colors); //'red', 'blue', 'green'
```

这种方法解决了原型链方法的一个痛点，即不能向超类构造函数传递参数：

```
function SuperType(age){
	this.age = age
}

function SubType(){
	SuperType.call(this, 10);
}

var instance = new SubType();
alert(instance.age); //10
```

## 局限性

所有的属性方法都得在构造函数中定义，实例、子类都无法访问到超类原型对象中的属性和方法，无法实现函数复用。我认为这种方法只是将超类中的属性和方法“复制”了一份放到子类中，缺少了一点灵活性。要想添加属性和方法就必须到目标构造函数中操作，当继承比较复杂时，添加起来可能比较麻烦。

# 组合继承

## 基本思想

组合继承，有时候也叫伪经典继承，指的是将原型链继承和构造函数继承的技术组合起来，从而发挥两种方法长处的一种继承模式。基本思路是使用原型链继承实现原型属性和方法的继承，用构造函数继承实现实例属性的继承。这样实例既能有自己的属性和方法，也能和其他实例共用原型函数上定义的方法。

## 实现

```
function SuperType(name){
	this.name = name;
	this.clolors = ['red', 'blue', 'green'];
}

SuperType.prototype.sayName = function (){
	alert(this.name);
}

function SubType(name, age){
	SuperType.call(this, name);
	this.age = 10;
}

SubType.prototype = new SuperType();
SubType.prototype.constructor = SubType;
SuperType.prototype.sayAge = function (){
	alert(this.age);
}

var instance1 = new SubType('haha', 10);
instance1.colors.push('black');
alert(instance1.colors); //'red', 'blue', 'green', 'black'
SubType.sayName(); //'haha'
SubType.sayAge(); //'10'

var instance2 = new SubType('hehe', 11);
alert(instance1.colors); //'red', 'blue', 'green'
SubType.sayName(); //'hehe'
SubType.sayAge(); //'11'
```

通过这种方式来实现继承，每个实例都可以拥有自己的属性和方法，同时通过继承定义在原型对象上的属性和方法实现复用，而且子类可以向父类传参，是现在最常用的方法。

## 局限性

先在子类内部调用超类构造函数，再重写子类的原型函数为超类的实例，这就导致超类构造函数中定义的属性和方法生成了两遍，即子类实例中的属性方法总会屏蔽掉子类原型对象上的属性和方法。

# 总结

再了解了三种基本的继承方法之后，对于js中继承的方式有了一个大体上的认识，下次将继续研究js中实现继承的其他方法，并比较其中优劣。我们下次见！
