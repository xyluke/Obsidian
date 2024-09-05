~~~js
function canParseJSON(str) {
    try {
        JSON.parse(str);
        return true; // 字符串可以解析为 JSON
    } catch (e) {
        return false; // 字符串无法解析为 JSON
    }
}
~~~


![[../img/Pasted image 20240905113259.png]]

~~~js
try {
let foo ={}
let bar = {
  a: {
    c: foo
  }
};
foo.b = bar;

JSON.stringify(foo)
} catch (e){
    console.log('JSON.序列化反应');
}
// 会报循环引用的错误，就会返回JSON序列化反应异常
~~~


```cardlink
url: https://blog.csdn.net/qq_34648151/article/details/119143921
title: "你会用 JSON.stringify()? JSON.stringify一些坑-CSDN博客"
description: "文章浏览阅读8.6k次，点赞7次，收藏14次。前言JSON是一种轻量级数据格式，可以方便地表示复杂数据结构。JSON对象有两个方法：stringify()和parse()。在简单的情况下，这两个方法分别可以将JavaScript序列化为JSON字符串，以及将JSON解析为原生JavaScript值。JSON.stringify MDN官方文档本文着重介绍JSON.stringify()的使用方法和注意事项。一、使用方法1、基本用法JSON.stringify()可以把一个JavaScript对象序列化为一个JSON字符串。let json1_json.stringify"
host: blog.csdn.net
```

