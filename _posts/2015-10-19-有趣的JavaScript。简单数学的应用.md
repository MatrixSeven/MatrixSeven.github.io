---
title: 有趣的JavaScript。简单数学的应用
date: 2015-10-19
toc: true
comments: true
tags: [js,数学,算法]
categories: JavaScript
---


<li>这个是2014年十一月份de在qq空间发布的。。。 搬到这里来。<br>
<li>2015年10月19日 发布在www.zhaoguilin.com在，最后迁移至此。<br>
<!--more-->
因为@R<br>
现在每当用到数学就想到曾经的。。
怀念完了，那就说说吧，首先这是我在google上面发现的一个写法。看了代码后觉得挺神奇的，就想了下，瞬间觉得数学屌爆了~

首先说，我们要实现的效果，就是判断鼠标进入和离开元素/元素的方向。
首先，一个普通人的想法应该是找到元素的边框坐标，然后进行判断。但是google到的这个JavaScript，仅仅用了一个高中数学小技巧及完美搞定了，而且给人一种高达上的感觉。
看JavaScript代码…

```javascript
$("#wrap").bind("mouseenter mouseleave",function(e) {
        var w = $(this).width();
        var h = $(this).height();
        var x = (e.pageX - this.offsetLeft - (w / 2)) * (w > h ? (h / w) : 1);
        var y = (e.pageY - this.offsetTop - (h / 2)) * (h > w ? (w / h) : 1);
        var direction = Math.round((((Math.atan2(y, x) * (180 / Math.PI)) + 180) / 90) + 3) % 4; //direction的值为“0,1,2,3”分别对应着“上，右，下，左”
        var eventType = e.type;
        var dirName = new Array('上方','右侧','下方','左侧');
        if(e.type == 'mouseenter'){
            $("#result").html(dirName[direction]+'进入');
        }else{
            $('#result').html(dirName[direction]+'离开');
        }
    });
```

对，就是这个公式：<br>

```JavaScript
var direction = Math.round((((Math.atan2(y, x) * (180 / Math.PI)) + 180) / 90) + 3) % 4;

```
实现了方向的判断.<br>
这真是一个巧妙的写法。<br>
<font color="red"> Math.atan2(y, x) 这句话首先拿到了鼠标进入点元素的点，然后使用atan2求出来这个点和元素中心所构成直线的反正切值，也就是所谓的弧度值、然后乘上了180 / Math.PI，很明显，这已经把弧度转换成了所对应的角度 ，但是由于Math.atan2(y, x) 的取值范围在-pi到pi之间，因此为了消除负数对结果的影响呢， 在公式后面加上了 180.转换后角度不再是-180~180，而是0~360，这样更加方便后面运算。最后 除以90然后加3在同 4取模，实现了和下方driName数组的对应，余数为0对应上方~
最后公式诞生了Math.round((((Math.atan2(y, x) * (180 / Math.PI)) + 180) / 90) + 3) % 4;
它的结果对应了数组new Array(‘上方’,’右侧’,’下方’,’左侧’);的下标。
</font><br>
很神奇，一个简单的高中数学知识，然你少写了N多的if~else~，而且看起来也变得高大上了~
哈哈。。
