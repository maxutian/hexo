---
title: Javascript中的继承方法(2)
date: 2017-08-04 17:08:20
tags:
	- javascript
	- 继承
	- 原型
	- 技术
categories: javascript
---

上次我们了解了js中的三种实现继承的方法，今天我们来把剩下的都说完！

<!-- more -->

# 原型式继承

## 基本思想

这种方法最早由道格拉斯.克罗克福德提出，主要思想是借助原型可以基于已有的对象创建新对象，同时还不必因此创建自定义类型。

## 实现

```
function object(o){
	function F(){}
	F.prototype = o;
	return new F();
}
```

在object()函数内部，先创建一个临时性的构造函数，再通过重写临时构造函数原型对象的方法实现继承，最后返回一个实例对象。

```
var person = {
	name: 'ma',
	friends: ['m', 'x', 't']
};

var anotherPerson = object(person);
anotherPerson.name = 'xu';
anotherPerson.friends.push('rob');

var yetAnotherPerson = object(person);
yetAnotherPerson.name = 'tian';
yetAnotherPerson.friends.push('bob');

alert(person.friends); //'m', 'x', 't', 'rob', 'bob';

```

要实现这种继承，要求你必须有一个对象可以作为另一个对象的基础，即例子中的person对象。当调用object函数返回新对象后，你可以根据自己的需要添加属性、方法。事实上，在es5中，新增的Object.create()方法起到了与object()方法相同的效果。所以我们可以用`Object.create(person)`语句来替换`object(person)`语句。

## 局限性

依然是引用类型数值的坑，即一个子类对引用类型值的修改会体现在所有子类中。并且函数不可复用。

# 寄生式继承

## 基本思想

寄生式继承的思路与寄生构造函数和工厂模式类似，即创建一个仅用于封装集成过程的函数，该函数在内部以某种方式来增强对象，最后返回增强过后的对象。

## 实现

```
function object(o){
	function F(){}
	F.prototype = o;
	return new F();
}

function createAnother(original){
	var clone = object(original);
	clone.sayHi = function(){
		alert('hi');
	};
	return clone;
}
```

注意，这里的object函数其实不是必须的，任何可以返回对象的函数都可以。这种方法在原型式继承的基础上再进行了一次封装，并同时添加属性或方法，最后的效果其实和原型式继承一样，只不过封装的更严密。

## 局限性

依然没有解决基础对象上引用类型值的共享问题。函数依然无法复用。

# 寄生组合式继承

## 思想

前面我们说到，组合式继承是目前使用最广的方法，但是其有一个弊端，即超类的实例属性会同时存在于子类的构造函数和原型对象内，这就会造成内存的浪费。所以，为了解决这一问题，诞生了寄生组合式继承，这种方法的思路是：依然使用在类中调用超类构造函数的方法来继承超类的实例属性，但是在继承原型对象上的属性时，使用寄生式方法来实现。

## 实现方式

```
function object(o){
	function F(){}
	F.prototype = o;
	return new F();
}

function inheritPrototype(subType, superType){
	var prototype = object(superType.prototype);
	prototype.constructor = subType;
	subType.prototype = prototype;
}

function SuperType(name){
	this.name = name;
	this.clolors = ['red', 'blue', 'green'];
}

SuperType.prototype.sayName = function (){
	alert(this.name);
}

function SubType(name, age){
	SuperType.call(this, name);
	this.age = age;
}

SubType.prototype = inheritPrototype(subType, superType);
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

Perfect！完美的解决了属性重复的问题！

# 总结

关于js中继承的实现方法算是总结完了，在对各种方法的了解过程中，知道了每种方法的思路与优劣，虽然我现在还没能把这些熟练的运用到实际代码中，但我相信这对于我以后的工作会有很大帮助，至少可以愉快的看别人的源码了！
