# 场景

利用gzip打包压缩后当前首页的大小
![[../../img/Pasted image 20240624101507.png]]



利用`webpack-bundle-analyzer` 分析出来的结果
![[../../img/Pasted image 20240624101539.png]]

*问题*
将传输的3.9MB能够把首页不太需要的文件都进行异步加载。不再首页全部加载。最好能够优化到1MB

*尝试*
将three.module.js和xlsx.core.min.js异步加载
![[../../img/Pasted image 20240624104847.png]]


![[../../img/Pasted image 20240624104930.png]]
![[../../img/Pasted image 20240624104952.png]]


![[../../img/Pasted image 20240624105612.png]]![[../../img/Pasted image 20240624105623.png]]
要命令行打开管理员模式才可以

![[../../img/Pasted image 20240624105757.png]]


![[../../img/Pasted image 20240625145040.png]]
用cdn