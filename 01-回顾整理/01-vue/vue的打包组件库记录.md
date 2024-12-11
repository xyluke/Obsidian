# 场景
vue有内部的hydra-web-component组件库，公司内部的。具体实现的步骤
![[../../img/Pasted image 20241211151209.png]]

build的语句解释
![[../../img/Pasted image 20241211151240.png]]

会把index.js打包成能用的文件。


# 打包成两部分
一、组件库的自动注册
~~~js

~~~

二、通用的工具类函数
# vue的use使用install
内部使用Vue.use的时候就会调用install方法
~~~js
// testInstall.js中
let install = function(v, param) {
    console.log("Test===============>", v, param);
}

export default {
    install
}

// main.js中
import Test from "./testInstall.js";
Vue.use(Test)
~~~