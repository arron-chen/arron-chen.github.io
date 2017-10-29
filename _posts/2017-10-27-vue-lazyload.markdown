---
layout: post
title: "Vue-lazyload 插件使用指南"
data: 2017-10-27
author: "arron"
header-img: "img/vue.png"
tags: 
  - note
---

### 前言 ###

> 当网络请求比较慢的时候,提前给这张图片添加一个像素比较低的占位图片，不至于堆叠在一块，或显示大片空白，达到更好的用户体验。

vue-lazyload 插件 [传送门]('https://www.npmjs.com/package/vue-lazyload')

### Installation 安装方式 ###

> npm

``` node
$ npm i vue-lazyload -D
```

> CDN

``` js
<script src="https://unpkg.com/vue-lazyload/vue-lazyload.js"></script>
<script>
  Vue.use(VueLazyload)
  ...
</script>
```
### 简单实例 ###

> 简单接入示例

html代码
``` html 
<div id="app">
    <li v-for="img in imgList">
        <img v-lazy="img">
    </li>
</div>
```
js代码：
``` js
<!-- 先引入 Vue -->
<script src="../js/vue.js"></script>
<!-- 引入组件库 -->
<script src="../js/index.js"></script>
<script>
    Vue.use(Lazyload);
    new Vue({
        el: '#app',
        data: {
            imgList: ['img url', 'img url', 'img url']
        }
    });
</script>
```
### 原理简述 ###

1. vue-lazyload是通过指令的方式实现的，定义的指令是v-lazy指令
2. 指令被bind时会创建一个listener，并将其添加到listener queue里面， 并且搜索target dom节点，为其注册dom事件(如scroll事件)
3. 上面的dom事件回调中，会遍历 listener queue里的listener，判断此listener绑定的dom是否处于页面中perload的位置，如果处于则加载异步加载当前图片的资源
4. 同时listener会在当前图片加载的过程的loading，loaded，error三种状态触发当前dom渲染的函数，分别渲染三种状态下dom的内容

### 用法 ###

main.js 在入口文件
``` js
import Vue from 'vue'
import App from './App.vue'
import VueLazyload from 'vue-lazyload'  //引入这个懒加载插件

Vue.use(VueLazyload)

// 或者添加VueLazyload 选项
Vue.use(VueLazyload, {
  preLoad: 1.3,
  error: 'dist/error.png',
  loading: 'dist/loading.gif',
  attempt: 1
})

new Vue({
  el: 'body',
  components: {
    App
  }
})
```
在入口文件添加后，在组件任何地方都可以直接使用把 img 里的:src -> v-lazy
``` html
 <div class="pic">
    <a href="#"><img :src="'/static/img/' + item.productImage" alt=""></a>
</div>
```
把之前项目中img 标签里面的 :src 属性 改成 v-lazy
``` html
<div class="pic">
    <a href="#"><img v-lazy="'/static/img/' + item.productImage" alt=""></a>
</div>
```

另外可以定制所有加载中和加载失败加载成功的样
``` css
<style>
 img[lazy=loading] {
  /*your style here*/
 }
 img[lazy=error] {
  /*your style here*/
 },
 img[lazy=loaded] {
  /*your style here*/
 }
 /*
 or background-image
 */
 .yourclass[lazy=loading] {
  /*your style here*/
 }
 .yourclass[lazy=error] {
  /*your style here*/
 },
 .yourclass[lazy=loaded] {
  /*your style here*/
 }
</style>
```

### 参数选项说明 ###

| key | description | default | options |
| --- | :---: | :---: | :---: |
| `preLoad` | proportion of pre-loading height | `1.3` | `Number` |
| `error` | 当加载图片失败的时候 | `'data-src'` | `String` |
| `loading` | 当加载图片成功的时候 | `'data-src'` | `String` |
| `attempt` | 尝试计数 | `3` | `Number` |
| `listenEvents` | 想要监听的事件 | `['scroll', 'wheel', 'mousewheel', 'resize', 'animationend', 'transitionend', 'touchmove']'` | Desired Listen Events |
| `adapter` | 动态修改元素属性 | `{ }` | Element Adapter |
| `filter` | 图片监听或过滤器 | `{ }` | Image listener filter |
| `lazyComponent` | lazyload component | `false` | Lazy Component |
| `dispatchEvent` | 触发dom事件 | `false` | `Boolean` |
| `throttleWait` | throttle wait | `200` | `Number` |
| `observer` | use IntersectionObserver | `false` | `Boolean` |
| `observerOptions` | 	IntersectionObserver options | `{ rootMargin: '0px', threshold: 0.1 }` | IntersectionObserver |


### 事件监听 ###

可以通过传递数组来配置想要使用vue - lazyload的事件
监听器的名字。
``` js
Vue.use(VueLazyload, {
  preLoad: 1.3,
  error: 'dist/error.png',
  loading: 'dist/loading.gif',
  attempt: 1,
  // the default is ['scroll', 'wheel', 'mousewheel', 'resize', 'animationend', 'transitionend']
  listenEvents: [ 'scroll' ]
})
```

---
> 每日一记

by the way ,胖真的是原罪。 好好锻炼身体，工伤致残就不好了！









