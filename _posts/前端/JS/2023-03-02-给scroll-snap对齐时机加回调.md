---
categories: [前端, JS]
tags: [JS]
---

## 问题描述

`scroll-snap`在展示一些滚动元素的时候非常有用，能做出一些类似 swiper 轮播的效果。
但

> 详细关于`scroll-snap`的介绍点这里 👉🏻 [传送门]({% post_url 前端/CSS/2023-03-01-CSS属性学习 %})

## 解决方案

### 方案一

简单无脑的笨方法
监听滚动事件，记录上一次滚动事件触发时的位置和当次做比较，如果位置没动则滚动结束，执行回调

```javascript
// 定时器，用来检测水平滚动是否结束
var timer = null;
// 上一次滚动的距离
var scrollLeft = 0,
  scrollTop = 0;
// 滚动事件开始
container.addEventListener("scroll", function () {
  clearInterval(timer);
  // 重新新的定时器
  timer = setInterval(function () {
    if (
      container.scrollLeft == scrollLeft &&
      container.scrollTop == scrollTop
    ) {
      // 滚动距离相等，认为停止滚动了
      clearInterval(timer);
      // ... 做你想做的事情，如回调处理
    } else {
      // 否则，依然记住上一次滚动位置
      scrollLeft = container.scrollLeft;
      scrollTop = container.scrollTop;
    }
  }, 100);
});
```

### 方案二

类似实现移动端双击效果的方法
监听滚动事件，每次触发事件都先清计时器，再建新的计时器，只有最后一次滚动事件触发，后面不会再清除计时器了，计时器内的回调才会执行，计时器的时间小于滚动事件的触发间隔即可

```javascript
// 定时器，用来检测水平滚动是否结束
var timer = null;
// 滚动事件开始
container.addEventListener("scroll", function () {
  clearTimeout(timer);
  // 重新新的定时器
  timer = setTimeout(function () {
    // 无滚动事件触发，认为停止滚动了
    // ... 做你想做的事情，如回调处理
  }, 100);
});
```

### 方案三

检测滚动区域子元素
监听滚动事件，获取*所有子元素距离视窗边缘的距离*和*滚动容器距离视窗边缘的距离*作比较，那个元素和容器的一样，就是哪个元素到位了

```javascript
[].slice.call(container.children).forEach(function (ele, index) {
  if (
    Math.abs(
      ele.getBoundingClientRect().left - container.getBoundingClientRect().left
    ) < 10
  ) {
    // 此刻的ele元素就是当前定位的元素
    // ... 你可以对ele做你想做的事情
  } else {
    // 此刻的ele元素不是当前定位的元素
  }
});
```
