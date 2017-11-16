---
layout:     post
title:      "Hello world,Hello my blog!"
subtitle:   "blogs, markdown, life"
date:       2017-09-14
author:     "arron"
header-img: "img/post-bg-js-version.jpg"
tags:
    - blog
    - life
---
> "Hello world,hello coder"

### 前言
  arron 的 blog 就这样开通了。

  [好事多磨，先上点干货吧](#build)

 
  


2017 年，迟到两年多的 blog 总算是出来了。


作为一个程序员， Blog 这种东西就像是你的名片，是好是坏一看便知，而一个善于经营自己的 developer 知道拿出自己的作品集，秀出来才是实实在在的！


<p id = "build"></p>
---

### 正文

接下来说说搭建这个博客的技术细节。  

在之前就有关注过 [GitHub Pages](https://pages.github.com/) + [Jekyll](http://jekyllrb.com/) 快速 Building Blog 的技术方案，非常轻松时尚。

其优点非常明显：

* **Markdown** 带来的优雅写作体验
* 非常熟悉的 Git workflow ，**Git Commit 即 Blog Post**
* 利用 GitHub Pages 的域名和免费无限空间，不用自己折腾主机
	* 如果需要自定义域名，也只需要简单改改 DNS 加个 CNAME 就好了
* Jekyll 的自定制非常容易，基本就是个模版引擎

哈哈，是不是很简单呢。其实只要多尝试几次，遇到的问题[geogle]()+[百度](https://baidu.com) 完全可以解决的

这样，就可以搭建好这样一个个人博客了

---
配置的过程中遇到一些坑，给大家抛出来看看


大的 Jekyll 主题上直接 fork 了 hux Blog（主题看起来相当不错）

本地调试环境需要 `gem install jekyll`，windows环境下操作相当简单，先下载对应电脑操作系统的 `ruby`，它自带`gem`， 可以直接`gem install jekyll`





另外考虑中文字体的渲染，fork 了 [Type is Beautiful](http://www.typeisbeautiful.com/) 的 `font` CSS，调整了字号，适配了 Win 的渣渲染，中英文混排效果好多了。

### 后记

回顾这个博客的诞生，纯粹是出于程序员不得不在乎的脸面吧。没有属于自己的 blog 就感觉是只野生程序猿没有`ID card`


哈哈哈哈，不管怎样，现在总是迈出了 blog 的第一步，以后再进行更多的优化和调整。

如果你恰好逛到了这里，希望你也能喜欢这个博客主题。

—— arron 后记于 2017.9