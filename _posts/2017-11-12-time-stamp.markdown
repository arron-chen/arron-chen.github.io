---
layout: post
title: "js转换时间戳"
author: 'arron'
date: "2017-11-12"
header-img: "img/post-bg-js-version.jpg"
tags: 
 - note
 
---

> 使用示例

``` js
    //传入10位时间戳格式数据e
    var time=new Data(e*1000)
    time.format("yyyy-mm-dd");
    
    //使用方法
    // var now = new Date();
    // var nowStr = now.format("yyyy-MM-dd hh:mm:ss");
    //使用方法2:
    // var testDate = new Date();
    // var testStr = testDate.format("yyyy年MM月dd日hh小时mm分ss秒");
    // alert(testStr);
    //示例：
    // alert(new Date().format("yyyy年MM月dd日"));
    // alert(new Date().format("MM/dd/yyyy"));
    // alert(new Date().format("yyyyMMdd"));
    // alert(new Date().format("yyyy-MM-dd hh:mm:ss"));

    
```

``` js
//格式化日期时间格式
Date.prototype.format = function(format) {
    var date = {
        "M+": this.getMonth() + 1,
        "d+": this.getDate(),
        "h+": this.getHours(),
        "m+": this.getMinutes(),
        "s+": this.getSeconds(),
        "q+": Math.floor((this.getMonth() + 3) / 3),
        "S+": this.getMilliseconds()
    };
    if (/(y+)/i.test(format)) {
        format = format.replace(RegExp.$1, (this.getFullYear() + '').substr(4 - RegExp.$1.length));
    }
    for (var k in date) {
        if (new RegExp("(" + k + ")").test(format)) {
            format = format.replace(RegExp.$1, RegExp.$1.length == 1
                ? date[k] : ("00" + date[k]).substr(("" + date[k]).length));
        }
    }
    return format;
}

//时间戳转换时间日期格式
function Dateformater(e){
    if(e){
        var newDate = new Date();
        newDate.setTime(e * 1000);
        var format = newDate.format('yyyy-MM-dd');
        return format;
    }
}
```

> js转换时间戳函数，留着自用。
