---
title: Music Player(2)
date: 2017-04-25 20:48:18
tags:
	- music player,
	- javascript,
	- audio
categories: javascript
---

这次主要是完善前段时间写的播放器，照例先放上项目地址：

{% note primary %}

**[项目地址](https://github.com/maxutian/js-practices/tree/master/player)**
**[在线预览](https://maxutian.github.io/js-practices/player/player.html)**

{% endnote %}

上一次，我已经实现了音乐播放器的大部分功能，包括：播放、暂停、进度条、音量控制、进度条和音量条的拖拽。由于这两天课比较多，这次只完成了点击改变进度的功能。由于上次已经完成了播放器的大部分内容，框架已经比较成熟，因此添加这么个功能就很简单了。

下面就是此部分功能的代码：

```

  function clickChange (duration) {
    var e = event || window.event;
    duration.onclick = function () {
      var dleft = event.clientX - duration.offsetLeft;
      if (this.id === 'v-player-duration') {
        var clickTime = (dleft / bgWidth.offsetWidth) * audio.duration;
        audio.currentTime = clickTime;
      }else if (this.id === 'v-volume-duration') {
        var clickVolume = dleft / vbgWidth.offsetWidth;
        audio.volume = clickVolume;
      }
      var currentWidth = (audio.currentTime / audio.duration) * bgWidth.offsetWidth;
      var currentVolume = audio.volume * vbgWidth.offsetWidth;
      ppb.style.width = ppointer.style.left = currentWidth + 'px';
      vpb.style.width = vpointer.style.left = currentVolume + 'px';
    }
  }

```

实现的思路基本上和拖动操作一样，用点击事件的clientX属性减去进度条的offseLeft属性，得出点击时鼠标距进度条左边界的距离。随后计算并设置pointer、progressbar的left、width属性，改变currenttime的值，完成点击改变进度的功能。

完成这一功能的逻辑很简单，运算也不复杂，但完成的过程中还是遇到了坑。比如一开始的时候，在改变进度条进度时，用的是百分比单位，但是发现在点击进度条时，最终位置会大于目标位置，并且越接近进度条终点越明显，而再我改成px单位后，就没有这个问题。于是一个坑就这么被莫名其妙的解决了。。。然而经过一番测试，我又发现了一个bug：点击音量条靠近末端的位置时，控制台会有音量大于1的报错，也就是说音量条的部分多出了一截，我认为可能是音量条的margin-left属性影响了offsetleft的值，于是试着修改，发现没有效果，在加上昨晚睡眠不好，只能暂且作罢，留着以后填坑！