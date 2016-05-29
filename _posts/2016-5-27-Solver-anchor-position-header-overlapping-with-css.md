---
layout: post
title:  "使用CSS解决链接锚点定位偏移的问题"
date:   2016-5-27 09:59:45
tags:	[Web Development, CSS ]
categories: [Web Development]
comments: false
---

在使用Github和Jekyll部署个人博客的时候，当增加树形目录（Table of content，TOC）时，锚点跳转的位置正好被Bootstarp的顶部导航栏遮挡住了一部分，从而导致在使用TOC的时候跳转的时候显示的位置有一定的偏差。 

<!-- more -->

## 锚点定位偏移问题描述
当我们为博客设置了一个固定位置的导航栏时，Markdown为页面其他内容生成的锚链接，即TOC结构。当点击锚链接时，页面就会跳转到相应的位置，且该锚链接的起始位置会缩进到最上方，从而导致部分内容由于被导航栏遮住而不能正常显示。如下表所示：

```
  点击：http://foo.com/#bar
  错误 (but the common behavior):         正确:
  +---------------------------------+      +---------------------------------+
  | BAR///////////////////// header |      | //////////////////////// header |
  +---------------------------------+      +---------------------------------+
  | Here is the rest of the Text    |      | BAR                             |
  | ...                             |      |                                 |
  | ...                             |      | Here is the rest of the Text    |
  | ...                             |      | ...                             |
  +---------------------------------+      +---------------------------------+
```

然而，我们实际期望的结果应该如右图所示。为解决这一问题，上网查了一下，发现大致有三种解决方法：

### 1.生成一个伪锚链接

在TOC脚本生成header的时候，为其添加一个div的class，

~~~html
<h1><a class="anchor" name="barlink">Bar</a></h1>
~~~

然后，在css中设置样式：

```css
/* 具体padding高度可以根据你导航栏的高度来设定 */
.anchor{padding-top:50px;}
```

事实上，这个方法困扰我挺长时间，然后也没有实现锚链接的偏移效果。

### 2.巧用margin和padding解决问题

首先，尝试给“文章”添加外边距，

```
margin-top:50px;
```

到这里，可以发现并没有任何的变化，由此可以得出结论：锚点定位跟外边距没有关系。

随后，给“文章”添加内边距，

```
padding-top:50px;
```

瞬间感觉整个世界都好了，但是锚点定位虽然准了，但是会多出来内边距部分。由此可知，锚点定位跟内边距有关。
最后，使用margin与padding相结合

```
padding-top:50px;
margin-top:-50px;
```

来解决锚点定位和多出来的内边距问题。`本博客就是使用这种方法来解决的问题`。

### 3.使用JS跳转来修正高度
这种方法的主要思路就是每次点击TOC中的目录时，都相应地往下滚动一定的高度，

```js
var shiftWindow = function() { scrollBy(0, -50) };
if (location.hash) shiftWindow();
window.addEventListener("hashchange", shiftWindow);
```

```cpp
class House{
	private:
	int a;
}
```

```c++
class House{
	private:
	int a;
}
```

这种方法，虽然也能解决问题，也在本博客上加以验证，但是稳定性欠佳，有时甚至无法实现正确的跳转。

## 修改结果截图
![](../../../res/201605/2016052901.png)