---
title: 初试css动画
date: 2017-05-05 20:20:35
tags:
	- css,
	- Animation
categories: CSS
---

{% note primary %}

**[项目地址](https://maxutian.github.io/practices/css%20practices/IT%20home%20animation/index.html)**

{% endnote %}

## 前言

作为一名IT之家基佬，闲来没事就刷一会，由于宿舍网络间歇性抽风，有时就会在加载界面停留很久,就看到三个红色的圆转啊转的。看着看着，感觉还蛮好看的，就想着用css动画做出类似的效果。刚好自己对css动画了解的不多，也可以趁机学习一下，于是就有了这篇文章。

## 分解

首先，在断网条件下，打开一个IT之家的页面，以尽可能长时间的观察动画效果。几次观察后，我首先确定了该动画的主体由三个红色的实心圆组成(/ □ \)。三个圆各占据一个等边三角形的顶点，先同时向中间靠拢直到相切，随后运动到一个相邻顶点的位置，完成一次循环。再完成对原动画的分解后，就可以着手进行还原了。

## 结构还原

首先，我们要先整出三个红色实心圆出来：

### HTML部分

```

	<div class="container">
		<div class="dot1"></div>
		<div class="dot2"></div>
		<div class="dot3"></div>
	</div>

```

### CSS部分

```

	body{
		display: flex;
		justify-content: center;
		align-items: center;
		height: 100vh;
		padding: 0;
		margin: 0;
	}
	.container{
		display: flex;
		flex-flow: row wrap;
		justify-content: space-between;
		width: 100px;
		height: 89.28px;
	}
	.dot1,.dot2,.dot3{
		width: 20px;
		height: 20px;
		border-radius: 50%;
		background-color: #e9382a;
	}
	.dot1,.dot3{
		align-self: flex-end;
	}

```

<!-- more -->

由于三个圆的圆心相连组成一个等边三角形，container的宽度是100，从所以容易得出container的最小高度为89.28px。以任意两个三角形顶点为端点，画1/6圆弧，当三圆圆心位于弧的中点时，三圆相切，此时容易算出三个圆的圆心相对于初始位置的相对位置，为下面设置动画的关键帧提供数据。说是容易，但是作为一个高中知识差不多忘光的数学渣，可花了我不少功夫呢。

## 动画还原

由于是第一次亲自动手写css动画，一开始还闹了不少笑话，比如@keyframes我写成了@keyframs，愣是半天发现，结果到了最后看效果的时候，动画特么的不动啊，搞的我差点崩溃。这也告诉了我一个道理：作为男人，该粗的时候粗，但写代码的时候一定要细。

![纯洁](https://www.tuchuang001.com/images/2017/05/05/ubiaoqing57c55ab4bc94612484.jpg)

其实，css动画的语法还是很简单的，再加上第二步计算得出的数据，很容易还原出原动画的效果：

```

	.dot1{
		animation-name: circle1;
		animation-duration: 1.5s;
		animation-timing-function: cubic-bezier(0.785, 0.000, 0.245, 1.005); 
		animation-iteration-count: infinite;
	}
	.dot2{
		animation-name: circle2;
		animation-duration: 1.5s;
		animation-timing-function: cubic-bezier(0.785, 0.000, 0.245, 1.005);
		animation-iteration-count: infinite;
	}
	.dot3{
		animation-name: circle3;
		animation-duration: 1.5s;
		animation-timing-function: cubic-bezier(0.785, 0.000, 0.245, 1.005);
		animation-iteration-count: infinite;
	}

	@keyframes circle1{
		0% {
			transform: translate(0px,0px);
		}
		50% {
			transform: translate(29.28px,-27.92px);
		}
		100% {
			transform: translate(40px,-69.28px);
		}
	}
	@keyframes circle2{
		0% {
			transform: translate(0px,0px);
		}
		50% {
			transform: translate(10.72px,41.36px);
		}
		100% {
			transform: translate(40px,69.28px);
		}
	}
	@keyframes circle3{
		0% {
			transform: translate(0px,0px);
		}
		50% {
			transform: translate(-40px,-10.72px);
		}
		100% {
			transform: translate(-80px,0px);
		}
	}

```

最后看看效果，基本上和原效果无甚差别，那么这个简单的动画就算是完成了！