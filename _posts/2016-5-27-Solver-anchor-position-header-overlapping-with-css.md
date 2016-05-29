---
layout: post
title:  "使用CSS解决链接锚点定位偏移的问题"
date:   2016-5-27 09:59:45
tags:	[Web Development, CSS ]
categories: [Web Development]
comments: false
---

在使用Github和Jekyll部署个人博客的时候，当增加树形目录（Table of content，TOC）时，会出现当点击TOC上的标题时，所跳转的位置并不是我们理想的位置，主要是因为Bootstarp的顶部包含一个导航条，锚点跳转的位置正好被导航条给遮挡住了一部分，从而导致在使用TOC的时候，有点强迫症的感觉。为此，花了很长的时间才解决，现在记录一下。 

<!-- more -->

## 锚点定位偏移描述