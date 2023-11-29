IOS的方法
> iOS11新增了两个 CSS函数 env、constant，用于设定安全区域与边界的距离。

3. 使用
（1）前提：H5网页设置viewport-fit=cover的时候才生效，小程序里的viewport-fit默认是cover。
（2）函数内部的预定义变量

safe-area-inset-left：安全区域距离左边边界的距离
safe-area-inset-right：安全区域距离右边边界的距离
safe-area-inset-top：安全区域距离顶部边界的距离
safe-area-inset-bottom：安全区域距离底部边界的距离

注意点`constant 和 env 同时使用的情况下，constant函数要在env函数的上面`

~~~css
.content{
	padding-bottom: constant(safe-area-inset-top: 20px); /* 兼容 iOS<11.2 */
    padding-bottom: env(safe-area-inset--top: 20px); /* 兼容iOS>= 11.2 */
    padding-bottom: constant(safe-area-inset-bottom: 20px); /* 兼容 iOS<11.2 */
    padding-bottom: env(safe-area-inset-bottom: 20px); /* 兼容iOS>= 11.2 */
 }

~~~
>https://juejin.cn/post/7290800838623854648
>https://blog.csdn.net/weixin_45811256/article/details/124884303
>https://blog.51cto.com/u_16213630/7855916


[安全区问题](https://blog.csdn.net/m0_37285193/article/details/119936097)

[适配举列子](http://www.yilingsj.com/xwzj/2020-07-26/iPhoneX-safe-area-inset-bottom.html)


