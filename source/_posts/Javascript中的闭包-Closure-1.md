---
title: Javascript中的闭包(Closure)
date: 2017-08-06 18:15:17
tags:
	- javascript
	- 闭包
	- 面试重点
categories: javascript
---

# 关于闭包

说到闭包，可以说是Javascript中的重点，在js中有着无比广泛的应用。从保留外部函数的变量对象，到实现公有、私有、特权变量都有他的身影。今天我们就来简单的聊聊闭包。

关于闭包的定义，《Javascript高级程序设计》中是这么说的：

{% note primary %}

闭包是指有权访问另一个函数作用域中的变量的函数。

{% endnote %}

我们都知道，在函数中定义的变量从外部是无法访问的，但是闭包可以，那么他是怎么做到的呢？

<!-- more -->

# 闭包原理

首先，在我们创建函数时，会预先创建一个包含全局变量对象的作用域链，当我们调用函数时，再将函数本身的变量对象添加到作用域链的最前端。然后当我们在函数中再声明一个函数(即闭包函数)，他会将外部函数的变量对象添加到他的作用域链中，那么他就可以通过作用域链访问到外部函数中的属性了。

不止于此，原先当我们调用函数时，当函数执行完毕，他的作用域链和变量对象就会销毁，但是闭包可以改变这一默认行为。即外部函数的变量对象不会立即销毁，直到闭包函数被销毁。这可能会带来内存占用过多的问题，所以我们应该及时解除闭包函数的引用。

# 闭包与变量

在我学习闭包的过程中，经常在网上看到一些面试题，其中出现频率最高的大概长这样：

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title></title>
  <script type="text/javascript">
    function onMyLoad(){
      var arr = document.getElementsByTagName("p");
      for(var i = 0; i < arr.length;i++){
        arr[i].onclick = function(){
            alert(i);
        }
      }
    }
  </script>
</head>
<body onload="onMyLoad()">
  <p>产品一</p>
  <p>产品二</p>
  <p>产品三</p>
  <p>产品四</p>
  <p>产品五</p>
</body>
</html>
```

我们期望的结果是随意点击一个p标签，会弹出此标签的对应下标。但实际结果却是点击所有的p标签弹出的都是5。为什么会这样呢？因为在闭包函数中，使用到的i属性来自他的外部函数，并且始终指向外部函数的i属性。这就导致当外部函数的i属性发生变化时，所有指针指向这一属性的值都会发生变化，而当循环结束时，i的值为5，所以点击任意p标签都会弹出5。

那么如何解决这个问题呢，最简单的莫过于向匿名函数传参，参数为每次循环时i的值：

```
function onMyLoad(){
  var arr = document.getElementsByTagName("p");
  for(var i = 0; i < arr.length;i++){
    arr[i].onclick = (function(item){
        alert(item);
    })(i);
  }
}
```

这样的话，每次循环，弹出的值就来自于自己的变量对象值item了，就不会受外部函数i的值的影响。

# 总结

这篇文章简要的介绍了闭包的原理与作用，结合一个小例子介绍了闭包的实际作用，自己对闭包的理解算是更深刻了。