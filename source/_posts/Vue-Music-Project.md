---
title: Vue-Music_Project
date: 2017-06-19 17:34:10
tags:
	- vue.js,
	- music-player,
	- readME
categories: javascript
---

临近期末了，要忙的事情很多，算来有将近一个月没有更新博客了，正好今天忙完了，上来更新一下最近的成果——[VueMusic][vuemusic]。没错，一个由vue全家桶写成的音乐播放器。虽然在我之前已经有很多大神写过类似的东西了，现在写有点跟风的意思，但是fuck it!能学到东西才是最重要的。

[在线地址][online]

写这个东西，一是为了完成一项期末大作业，二是因为自己的兴趣，当然最重要的还是为了提高自己的技术。对于刚接触vue的一个js初学者来说，在甚至没有看完vue文档的情况下，能够完成这个项目，真的是相当的不容易。虽然这个项目目前只是“能用”而已，还有许多不完善的地方，但是别担心，我以后会继续完善这个项目的。


好了，下面是一个简短的README：

## 技术栈

* Vue.js 2.0
* VueX
* VueAxios
* VueRouter
* VueSlider
* VueMaterial
* NetEaseMusicApi

其中，[VueSlider][vs]用于实现播放器和音量控制的进度条部分,[VueMaterial][vm]是一个开源的UI框架，用于在vue项目中实现google的Material Design设计语言。

<!-- more -->

## 使用方法

首先，你需要下载这个项目和需要用到的api插件

``` bash 
git clone https://github.com/maxutian/vue-music.git
git clone git@github.com:Binaryify/NeteaseCloudMusicApi.git
```

然后分别安装依赖

``` bash
# install dependencies
npm install
```

分别将他们运行在本地

``` bash
npm run dev //vue-music项目
node app.js //api插件
```

## 注意事项

由于最近api接口出了点问题，导致无法获取封面图片，如果你很介意的话，可以通过安装一个chrome插件来解决，[插件下载地址][plugin]

## Todo List

* 正在播放提示
* 搜索功能
* 循环模式

最后，希望你们喜欢这个项目，如果能点个赞就更好了。

[vm]: https://github.com/vuematerial/vue-material
[vs]: https://github.com/NightCatSama/vue-slider-component
[plugin]: https://pan.baidu.com/s/1i5mmdSh
[vuemusic]: https://github.com/maxutian/vue-music
[online]: http://music.maxutian.cn