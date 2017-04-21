---
title: Music Player(1)
date: 2017-04-21 19:16:00
tags:
	- music player,
	- javascript,
	- audio
categories: javascript
---

本人的专业是数字媒体技术，这学期有一门课叫数字媒体作品制作，要求制作一个与专业相关的作品。由于大二下学期开过网页设计制作这门课，再加上近期对vue很着迷，就打算用vue制作一个pc端的音乐播放器。但是我以前并没有接触过vue，于是前一段时间，我边学边做，终于把播放器的大概框架搭好了。但是在制作player这个组件时，遇到了一点困难，就打算把这个组件单独拿出来做，到时再移植到vue中，在巩固js基础的同时，也更能了解类似vue这种MVVM框架的优点。

{% note primary %}

**[项目地址](https://github.com/maxutian/js-practices/tree/master/player)**

{% endnote %}

首先，我们需要用HTML中的各个标签搭建出项目的框架，然后用css美化之。在写css时，大量运用了前段时间学习的flexbox布局，感觉不要太爽！这部分内容比较基础，我就不再赘述了。如果你感兴趣，可以查看[源文件](https://github.com/maxutian/js-practices/blob/master/player/style/style.css)。

下图是完成后的样子：

![外观图](https://www.tuchuang001.com/images/2017/04/21/d382332e28bcc77b.png)

在正式动工之前，我先在mdn查看了audio标签的各种api，对于歌曲的播放、暂停，音量的控制方法等有了一个大致的了解。但是除了这些最基础的功能，还需要实现进度条随歌曲播放的滚动效果、拖动进度条改变歌曲进度/改变音量大小的功能，这也是这个player最复杂的部分。

下面就开始正式实现这个播放器。

播放器的播放、暂停功能通过一个按钮实现，我们首先给按钮绑定一个点击事件，当点击事件发生时，通过改变<i>标签的innerHTMl来改变按钮的图标。同时结合audio.play()、audio.pause()方法实现歌曲的播放和暂停。

```

function $ (ele){
  return document.querySelector(ele);
}
// 控制播放/暂停

var audio = $('#v-player-music');

function changeIcon(){
  var ppi = $('#play-pause-icon');
  var ppt = $('#play-pause-text');
  if (ppi.innerHTML === 'play_circle_outline'){
    ppi.innerHTML = 'pause_circle_outline';
    audio.play();
  }else{
    ppi.innerHTML = 'play_circle_outline';
    audio.pause();
  }
}

```

同样的原理，实现音量键的静音功能：

```

// 控制静音

function volumeControl () {
  var muted = $('voice');
  if (voice.innerHTML === 'volume_up') {
    voice.innerHTML = 'volume_off';
    audio.muted = true;

  } else {
    voice.innerHTML = 'volume_up';
    audio.muted = false;
  }
}

```

<!-- more -->

接下来，实现歌曲进度条和音量条的拖动功能，由于对相关api并不是特别了解，就从网上找了一个现成的轮子(由于进度条和音量条都需要实现拖动，我对原函数进行了封装，便于重复使用。另外把原来的固定长度改成了百分比长度，以解决窗口大小的改变造成的进度条长度不正确问题)：

```

// 实现进度条和音量条拖拽及控制

var container = $('#v-player-container');
var pduration = $('#v-player-duration');
var vduration = $('#v-volume-duration');
var ppointer = $('#pointer');
var vpointer = $('#v-pointer');
var bgWidth = $('#duration-bg');
var vbgWidth = $('#v-duration-bg');
var ppb = $('#progress-bar');
var vpb = $('#v-progress-bar');
var barleft = 0;
function Drag (pointer,duration,pb,Width) {
  pointer.onmousedown = function(){
    var event = event || window.event;
    var leftVal = event.clientX - this.offsetLeft;
    var that = this;
    document.onmousemove = function(){
      var event = event || window.event;
      barleft = event.clientX - leftVal;
      if(barleft < 0){
        barleft = 0;
      } else if(barleft > duration.offsetWidth - ppointer.offsetWidth) {
        barleft = duration.offsetWidth - ppointer.offsetWidth;
      }
      pb.style.width = (barleft / Width.offsetWidth) * 100 + '%';
      that.style.left = (barleft / Width.offsetWidth) * 100 + '%';
      window.getSelection ? window.getSelection().removeAllRanges() : document.selection.empty();
    }
  }
}
document.onmouseup = function () {
  document.onmousemove = null;
}

```

成功实现进度条的拖动！过程中，了解了以前没接触过的clientX、offsetWidth、offsetLeft等属性，在以后碰到类似的问题时，不至于毫无头绪了。

在真正实现拖动进度条功能之前，需要在进度条右方添加歌曲的总时间和当前播放时间。运用audio的duration属性，配合简单的乘除法和求余运算就可以实现：

```

// 获取歌曲长度和当前时间

var musicMinutes = $('#musicMinutes');
var musicSeconds = $('#musicSeconds');
var currentMinutes = $('#currentMinutes');
var currentSeconds = $('#currentSeconds');
function ms (num,position){
  if(num < 10){
    position.innerHTML = '0' + num;
  }else{
    position.innerHTML = num;
  }
}
function musicLength () {
  var mmin = Math.floor(audio.duration / 60);
  var msec = Math.floor(audio.duration % 60);
  ms(mmin,musicMinutes);
  ms(msec,musicSeconds);
}
function currentLength () {
  var cmin = Math.floor(audio.currentTime / 60);
  var csec = Math.floor(audio.currentTime % 60);
  ms(cmin,currentMinutes);
  ms(csec,currentSeconds);
}

```

接下来，就要通过进度条的拖拽实现歌曲进度的改变和音量大小的改变了。这里，我通过progressbar的width属性占进度条总长度的比例配合duration属性以确定进度条上不同位置对应的歌曲时间。

```

function Drag (pointer,duration,pb,Width) {
  pointer.onmousedown = function(){
    var event = event || window.event;
    var leftVal = event.clientX - this.offsetLeft;
    var that = this;
    document.onmousemove = function(){
      if(that.id === 'pointer'){
        // 改变歌曲进度
        var dragTime = (ppb.clientWidth / bgWidth.offsetWidth) * audio.duration;
        audio.currentTime = dragTime;
      }else if (that.id === 'v-pointer'){
        // 改变歌曲音量
        var dragVolume = vpb.clientWidth / vbgWidth.offsetWidth;
        audio.volume = dragVolume;
      }
      var event = event || window.event;
      barleft = event.clientX - leftVal;
      if(barleft < 0){
        barleft = 0;
      } else if(barleft > duration.offsetWidth - ppointer.offsetWidth) {
        barleft = duration.offsetWidth - ppointer.offsetWidth;
      }
      pb.style.width = (barleft / Width.offsetWidth) * 100 + '%';
      that.style.left = (barleft / Width.offsetWidth) * 100 + '%';
      window.getSelection ? window.getSelection().removeAllRanges() : document.selection.empty();
    }
  }
}
document.onmouseup = function () {
  document.onmousemove = null;
}

```

在这里，我碰到了好大一个坑，经过我的不懈努力，最终才算解决了。一开始，我将改变歌曲进度的代码放在了onmouseup事件下：

```

document.onmouseup = function () {
	var dragTime = (ppb.clientWidth / bgWidth.offsetWidth) * audio.duration;
  audio.currentTime = dragTime;
  document.onmousemove = null;
}

```

结果发现，不管在页面任意位置点击鼠标，都会获取一次当前时间，这会导致歌曲的播放出现类似于“卡带”的问题。为了解决这一问题，我先尝试将代码放在onmousemove事件下，但是在拖动音量条时，还是会导致“卡带”。于是我将document的onmouseup事件改成了ppointer(进度条上的圆形按钮)的onmouseup事件：

```

ppointer.onmouseup = function () {
	var dragTime = (ppb.clientWidth / bgWidth.offsetWidth) * audio.duration;
  audio.currentTime = dragTime;
}

```

但真正的坑来了，我发现虽然这一方法实现了我的预期效果，不管是点击界面或者是拖动音量条都不会导致卡带了。但是在拖动进度条时，每当进度条的长度超过某一临界点(大概是总长度的2/3)，就无法(或者说极其困难)再继续向前拖动了，而且整个拖动过程变得很不流畅，有时甚至得随缘才能完成拖动。我在chrome的开发者工具中各种打断点，调试了好久也没有解决。只好放弃这一方法。如果您看到了这篇文章并且有解决这一问题的方法的话，请留言，或者通过我的邮箱告诉我解决方法，我的邮箱是**maxutian95@gmail.com**万分感激！

于是，又回到了最初的起点。在思考良久后，我觉得还是应该把这段代码放在document.onmousemove下。同时，通过if语句，判断参数pointer的id，从而实现歌曲进度条和音量条的独立、互不影响。最后测试了一下，很完美，没有问题。

到这里，一个原生js实现的音乐播放组件就大部分完成了，但是还需要添加几个功能，比如点击进度条改变歌曲进度，以及上一曲和下一曲按钮的功能实现。这些我将在下一篇文章中逐一完成。与此同时，还要抽空学习vue的知识，毕竟是自己挖的坑，就一定要填完！
