---
layout: post
title: "Vue内置组件--transition 踩坑记"
data: 2017-10-26
author: "arron"
header-img: "img/vue1.png"
tags: 
    -note
---
# **vue**单元素/组件过渡实现

> 什么是过渡

Vue只有在插入，更新或者移除DOM元素时才会应用过渡效果，过渡效果的应用可以通过不同方式实现，官方文档中提到了如下几种：

 - 在CSS过渡和动画中自动应用class；
 - 配合使用第三方的CSS动画库，如Animate.css；
 - 在过渡钩子函数中使用JavaScript直接操作DOM；
 - 配合使用第三方JavaScript动画库，如Velocity；

 上面四种方式其实主要就是两种，一个是利用CSS过渡或者动画，另一个是利用JavaScript钩子函数。

 > 如何应用过渡到元素/组件上

 要想使元素或者组件应用到我们所写的过渡动画，需要使用vue提供的transition来封装组件成为过渡组件，transition需要与如下情景中的任一种一起使用：
  - v-if（条件渲染）
  - v-show（条件展示）
  - 动态组件
  - 在组建的根节点上，并且被vue实例DOM方法触发，如appendTo方法把组件添加到某个根节点上

  当需要插入或者删除封装成过渡元素的元素时，vue将做如下事情：

  - 查找目标元素是否有CSS过渡或者动画，如果有就在适当的时候进行处理；
  - 如果过渡组件设置了JavaScript钩子函数，vue会在相应阶段调用钩子函数；
  - 如果以上两者都没有，DOM操作（插入或者删除）就在下一帧立即执行。

## CSS过渡
 
举一个典型的CSS过渡的例子：

``` html
<!-- 首先将要过渡的元素用transition包裹，并设置过渡的name，然后添加触发这个元素过渡的按钮（实际项目中不一定是按钮，任何能触发过渡组件的DOM操作的操作都可以） -->
<div>
  <button @click="show=!show">show</button>
  <transition name="fade">
    <p v-show="show">hello</p>
  </transition>
</div>

```

``` css
// 接着为过渡类名添加规则
&.fade-enter-active, &.fade-leave-active
  transition: all 0.5s ease     
&.fade-enter, &.fade-leave-active
  opacity: 0 

```
封装上面的代码，就可以实现一个简单的动画了，CSS的transition属性是用来设置过渡总体效果的。

### CSS过渡类名

组件过渡过程中，会有四个CSS类名进行切换，这四个类名与上面transition的name属性有关，比如name="fade"，会有如下四个CSS类名：

 - fade-enter：进入过渡的开始状态，元素被插入时生效，只应用一帧后立即删除；
 - fade-enter-active：进入过渡的结束状态，元素被插入时就生效，在过渡过程完成之后移除；
 - fade-leave：离开过渡的开始状态，元素被删除时触发，只应用一帧后立即删除；
 - fade-leave-active：离开过渡的结束状态，元素被删除时生效，离开过渡完成之后被删除；

 从上面四个类名可以看出，fade-enter-active和fade-leave-active在整个进入或离开过程中都有效，所以CSS的transition属性在这两个类下进行设置。 
上面示例中，fade-enter和fade-leave-active类设置CSS为opacity:0，说明过渡刚进入和离开的时候透明度为0，即不显示。当然还可以设置其他的CSS属性，transform属性是除了opacity之外经常在这里被用到的，transform用法可参考 [传送门]('http://www.w3school.com.cn/cssref/pr_transform.asp')

### CSS动画

组件过渡的实现不仅可以通过CSS过渡还可以通过CSS动画(animation)实现，建议先了解一下CSS3 Animation，这里还是给个例子：

``` html
<div>
  <button @click="show=!show">show</button>
  <transition name="fold">
    <p v-show="show">hello</p>
  </transition>
</div>
```
``` css
.fold-enter-active {
  animation-name: fold-in;
  animation-duration: .5s;
}
.fold-leave-active {
  animation-name: fold-out;
  animation-duration: .5s;
}
@keyframes fold-in {
  0% {
    transform: translate3d(0, 100%, 0);
  }
  50% {
    transform: translate3d(0, 50%, 0);
  }
  100% {
    transform: translate3d(0, 0, 0);
  }
}
@keyframes fold-out {
  0% {
    transform: translate3d(0, 0, 0);
  }
  50% {
    transform: translate3d(0, 50%, 0);
  }
  100% {
    transform: translate3d(0, 100%, 0);
  }
}
```
如果预先了解了CSS动画（上面给了链接），上面代码还是很好理解的，要注意的是CSS动画中，fold-enter类名在节点插入DOM后不会立即删除，而是在animationed事件触发时删除。

### 自定义过渡类名

上面的四个过渡类名都是根据transition的name属性自动生成的，那么能否自己定义这四个类名呢？答案是可以的，通过enter-class、enter-active-class、leave-class、leave-active-class这四个特性来定义。

``` html
<div>
  <button @click="show=!show">show</button>
  <transition 
    name="fade"
    enter-class="fade-in-enter"
    enter-active-class="fade-in-active"
    leave-class="fade-out-enter"
    leave-active-class="fade-out-active"
  >
    <p v-show="show">hello</p>
  </transition>
</div>
```
``` css
&.fade-in-active, &.fade-out-active
  transition: all 0.5s ease     
&.fade-in-enter, &.fade-out-active
  opacity: 0 
```
上面代码中，原来默认的fade-enter类对应fade-in-enter，fade-enter-active类对应fade-in-active，依次类推。

## JavaScript钩子函数

除了用CSS过渡的动画来实现vue的组件过渡，还可以用JavaScript的钩子函数来实现，在钩子函数中直接操作DOM。我们可以在属性中声明以下钩子：

``` html 
<transition
  v-on:before-enter="beforeEnter"
  v-on:enter="enter"
  v-on:after-enter="afterEnter"
  v-on:enter-cancelled="enterCancelled"
  v-on:before-leave="beforeLeave"
  v-on:leave="leave"
  v-on:after-leave="afterLeave"
  v-on:leave-cancelled="leaveCancelled"
>
</transition>
```
``` js
methods: {
  // 过渡进入
  // 设置过渡进入之前的组件状态
  beforeEnter: function (el) {
    // ...
  },
  // 设置过渡进入完成时的组件状态
  enter: function (el, done) {
    // ...
    done()
  },
  // 设置过渡进入完成之后的组件状态
  afterEnter: function (el) {
    // ...
  },
  enterCancelled: function (el) {
    // ...
  },
  // 过渡离开
  // 设置过渡离开之前的组件状态
  beforeLeave: function (el) {
    // ...
  },
  // 设置过渡离开完成时地组件状态
  leave: function (el, done) {
    // ...
    done()
  },
  // 设置过渡离开完成之后的组件状态
  afterLeave: function (el) {
    // ...
  },
  // leaveCancelled 只用于 v-show 中
  leaveCancelled: function (el) {
    // ...
  }
}
```
上面的钩子函数中可以进行任何你想做的DOM操作。


> 小技巧：

如果你只想设置组件过渡进入的效果而不想有组件过渡离开的效果，这时你就可以用钩子函数，只设置beforeEnter、enter、afterEnter这几个钩子函数就可以了。

> 做项目遇到的坑，踩过。。。 就记着了


