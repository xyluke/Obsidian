场景: hmi webpack `^3.3.12`  升级为 `^5.65.0`


npm-check-updates, 新的全部版本更新查找依赖
~~~shell
ncu  // 查看全部包的最新接口

ncu vue // 查看单个包的最新版本

ncu -u // 更新所有的版本为最新

ncu -g //更新全局为最新的包

npm view <package-name> versions // 全部的可用版本号
~~~


错误1
![[img/Pasted image 20230817111547.png]]
![[img/Pasted image 20230817111635.png]]
~~~text

~~~


错误2
![[img/Pasted image 20230817112209.png]]

![[img/Pasted image 20230817112227.png]]
~~~text
loaders: ["style", "css", "sass"]  --> use: ["style-loader", "css-loader", "sass-loader"]
~~~



错误3
![[img/Pasted image 20230817113619.png]]
![[img/Pasted image 20230817113657.png]]
> 更新了`html-webpack-plugin` 版本号 `^2.30.1`  到 `^5.5.0`



错误4
![[img/Pasted image 20230817145552.png]]
> sass的错误
>  npm ri node-sass@4.14.1 sass-loader@7.3.1 --save-dev 安装这个适应版本的


错误5
![[img/Pasted image 20230817145943.png]]
> loader重复声明了
> [链接](https://www.cnblogs.com/wangtaolearning/p/10363947.html)
![[img/Pasted image 20230817173412.png]]



错误6
![[img/Pasted image 20230817173602.png]]
[链接](https://www.cnblogs.com/vickylinj/p/12787402.html)

~~~js
const { VueLoaderPlugin } = require('vue-loader');

module.exports = {
	.
	.
	.
	plugins: [
        new VueLoaderPlugin()
    ]
}
~~~


错误7 -build
![[img/Pasted image 20230817191341.png]]

> 将使用 `extract-text-webpack-plugin` 的 loader 替换为 `mini-css-extract-plugin` 的 loader



![[img/Pasted image 20230818085915.png]]
> node和sass的版本冲突，node14 最高的node-sass就是4.14+，而项目实际用的是7.0.3的node-sass。

![[img/Pasted image 20230818085953.png]]