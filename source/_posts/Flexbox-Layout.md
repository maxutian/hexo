---
title: Flexbox Layout
date: 2017-03-26 20:54:06
tags:
	-flexbox
	-css
categories: CSS
---

{% note primary %}

**flexbox布局学习笔记**
**[版权所属](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)**

{% endnote %}

flexbox布局虽然早已出现，但是作为前端菜鸟，接触的时间并不算长，使用的次数就更加屈指可数了。正好最近在折腾的一个项目需要频繁的使用flexbox布局，但是在使用的过程中，发现老是记不住其中的各种属性名，大大降低了开发的效率，于是下定决心做一篇学习笔记！

## 背景

flexbox布局模块主要是为了提供一个更加方便有效的布局方式，它将一个容器中的各个子项目区分开来、校准整齐。甚至是对于那些不知道大小的或者动态变化的项目，flexbox都能够完美生效，弹性布局的名称也因此而来。学会了flexbox布局，再也不用纠结如何清除恼人的浮动，再也不必为了使一个元素垂直居中翻箱倒柜，求助各种黑科技了！所以，从现在起拥抱flexbox吧！

<!-- more -->

## 基本知识

首先我们要知道，flexbox是一个完整的模块而不是一个单一的属性。它包含许多东西，我们在使用它时，需要将container上的属性和items（container中的项目）上的属性结合起来。

如果说传统布局是基于块级布局和行内布局的话，那么flexbox则是基于flexbox布局。那么到底什么是flexbox布局呢？下面这张图就解释了隐藏在flex布局后的主要思想：

![flexbox](https://cdn.css-tricks.com/wp-content/uploads/2011/08/flexbox.png)

基本上，flexbox中的项目将会沿着主轴（从main-start到main-end）或交叉轴（从cross-start到cross-end）进行排列。

- **main axis**
主轴。所有container中的项目都会默认按照主轴进行排列，但是注意，它的方向并不总是水平的，主轴的方向主要取决于flex-direction属性（见下文）。

- **main-start|main-end**
子项目在container中都是从main-start到main-end的方向排列的。

- **main size**
一个flexbox的子项目在主轴方向上占据的空间（项目的宽度或高度）。

- **cross axis**
交叉轴。与主轴垂直的轴叫做交叉轴，它的方向取决于主轴的方向。

- **cross-start|cross-end**
所有项目在flexbox中的交叉轴方向上总是从cross-start到cross-end排列。

- **cross size**
一个flexbox的子项目在交叉轴方向上占据的空间（项目的高度或宽度）。

{% note primary %}

**好了，在了解了基本的概念后，我们开始正式进入flexbox的世界！**

{% endnote %}

## 容器上的属性

![container](https://css-tricks.com/wp-content/uploads/2014/05/flex-container.svg)

### display

这个属性定义了一个flexbox容器，至于它是行内元素还是块级元素，则取决于给定的值。它给其中包含的所有子项目提供了一个flex环境。

```

.container{
	display: flex;
}

```

{% note danger %}

**注意：** css的多列布局在flex容器中没有效果

{% endnote %}

### flex-direction

![flex-direction](https://css-tricks.com/wp-content/uploads/2013/04/flex-direction2.svg)

这个属性建立了容器的主轴，从而定义了容器中子项目的排列方向。所有的子项目按照主轴的方向水平成行或者竖直成列。

```

.container{
	flex-direction: row | row-reverse | column | column-reverse;
}

```

- **row(默认):** 所有子项目从左到右排列，与当前浏览器语言方向相同。

- **row-reverse:** 与row相反，子项目会从右到左排列，并仍与浏览器的语言方向有关。

- **column:** 与row相似但是默认是按照从上到下的顺序排列的。

- **column-reverse:** 与row-reverse相似，但是按照从下往上的顺序排列。

### flex-wrap

![flex-wrap](https://css-tricks.com/wp-content/uploads/2014/05/flex-wrap.svg)

默认情况下，所有子项目都会试着挤在一行中。你可以改变这个属性从而使子项目能在需要的时候自动换行。

```

.container{
	flex-wrap: nowrap | wrap | wrap-reverse;
}

```

- **nowrap(默认):** 简洁明了，所有子项目只会在一行中排列。

- **wrap:** 子项目将会按照从上到下的顺序多行排列。

- **wrap-reverse:** 子项目按照从下到上的顺序多行排列。

### flex-flow

这是flex-direciton和felx-wrap的缩写形式，共同定义了flex容器的主轴和交叉轴，默认为row nowrap。

```

flex-flow: <'flex-direction'> | <'flex-wrap'>;

```

### justify-content

![justify-content](https://css-tricks.com/wp-content/uploads/2013/04/justify-content.svg)

这个属性定义了子项目在主轴上的布局方式。当所有处于同一行上的子项目都是“非弹性”的，或者虽然是“弹性”的但是达到了它们的最大尺寸。这个时候，这一属性就将发挥它的功效：将所有余下的空白空间合理区分开来。同时，当子项目超出行的范围时，这一属性也会对其起到控制的作用。

```

.container{
	justify-content: flwx-start | flex-end | cneter | space-between | space-around;
}

```

- **flex-start(默认):** 左对齐。

- **flex-end:** 右对齐。

- **center:** 水平居中。

- **space-between:** 两端对齐，各个项目间间隔相等。

- **space-around:** 每个子项目周围的空白空间都相等

### align-items

![align-items](https://css-tricks.com/wp-content/uploads/2014/05/align-items.svg)

这个属性定义了当前行中的所有子项目在交叉轴上的布局方式。

```

.container{
	align-items: flex-start | flex-end | center | baseline | stretch;
}

```

- **flex-start:** 顶端对齐。

- **flex-end:** 底部对齐。

- **center:** 垂直居中。

- **baseline:** 基线对齐。

- **stretch:** 每个子项目的高度等于flexbox的的高度。

### align-content

![align-content](https://css-tricks.com/wp-content/uploads/2013/04/align-content.svg)

当交叉轴方向上有多余空间时，这个属性定义了多行子项目在交叉轴上的布局方式，与justify-content原理相同。

{% note danger %}

**注意:** 当项目只有一行时，此属性无效！

{% endnote %}

```

.container{
	align-content: flex-start | flex-end | center | sapce-between | sapce-around | stretch;
}

```

- **flex-start:** 所有行向顶部靠齐。

- **flex-end:** 所有行向底部靠齐。

- **center:** 所有行(看作一个整体)垂直居中。

- **space-between:** 第一行和最后一行分别位于顶端和底端，其他所有行均匀排列。

- **space-around:** 所有行周围的空白空间相等。

- **stretch(默认):** 所有行的高度相加等于容器高度。

## 子项目属性

![flex-items](https://css-tricks.com/wp-content/uploads/2014/05/flex-items.svg)

### order

![order](https://css-tricks.com/wp-content/uploads/2013/04/order-2.svg)

默认情况下，子项目按照它们的原始顺序排列。order属性可以控制各个子项目在容器中的排列顺序，数值越小，排列越靠前。

```

.item{
	order: <integer>;
}

```

### flex-grow

![flex-grow](https://css-tricks.com/wp-content/uploads/2014/05/flex-grow.svg)

此属性接受一个非负数作为值(默认为0)，而整个容器会根据这个值为该子项目分配空间，如果所有子项目的值都为1，那么空间将被均匀分配，如果其中一个被设置成2，那么它所占据的空间将是其它子项目的两倍。

```

.item{
	flex-grow: <number>;
}

```

### flex-shrink

这个属性定义了子项目在必要时的缩小能力。

```

.item{
	flex-shrink: <number>;
}

```

此属性接受一个非负数作为值，默认为1，即当空间不足时，所有项目将缩小。如果有一个项目被设置为0，那么它将不会缩小。

### flex-basis

这个属性定义了一个项目在剩余空间被分配之前的默认大小。它可以是一个具体数值(比如20%、5rem等)，也可以是一个关键词。如果设置为auto(默认为auto)，那么它的大小取决于该项目的main-size大小(即该项目原本大小),而多余的空间将基于它的flex-grow属性进行分配。如果设置为content，那么项目大小将取决于项目中内容的大小(此属性暂时还未受到良好支持)。

```

.item {
  flex-basis: <length> | auto;
}

```

### flex

这个属性是flex-grow,flex-shrink和flex-basis三个属性的缩写。第二第三个属性(flex-shrink和flex-basis)可选，默认为0 1 auto。

```

.item {
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}

```

{% note primary %}

**在这里我们推荐使用这个简写属性而不是分别使用三个属性，因为当你设置其中一个属性时，其它两个属性会自动做出最优设置。**

{% endnote %}

### align-self

![align-self](https://css-tricks.com/wp-content/uploads/2014/05/align-self.svg)

这个属性将使一个单一的项目基于其原始布局做出不同的布局方式，你可以回顾align-items的部分以了解
以下可选值的含义。

```

.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}

```

{% note danger %}

**注意:** float,clear和vertical-align不会对任何一个子项目产生影响。

{% endnote %}

## 结语

终于从头到尾把flexbox梳理了一遍，对其中各种属性的理解更深刻了。以后脱稿写flexbox的样式应该是没问题了，javascript还在继续填坑中，继续加油吧！

{% note primary %}

第一次翻译英文文章，有翻译不好的地方还请多指教！

{% endnote %}