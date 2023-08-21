# 场景
项目需要，将node@14.15.4的版本升级为node@16.14.2。sass-loader和node-sass这两个依赖在升级中出现了版本不匹配的问题。

# 报错

![[img/Pasted image 20230818111718.png]]


![[img/Pasted image 20230818113007.png]]

# 原因
> node  node-sass和sass-loader的版本不匹配问题

node与node-sass版本匹配如下:
![[img/Pasted image 20230818142617.png]]

我收集到的集中版本配合
> node.js@16.13.2  node-sass@6.0.0  sass-loader@10.2.0
> node.js@14.15.4  node-sass@4.14.1 sass-loader@7.3.1 
> node.js@10.15.3  node-sass@5.0.0   sass-loader@10.1.1


sass-loader 4.1.1，node-sass 4.3.0
sass-loader 7.0.3，node-sass 4.7.2
sass-loader 7.3.1，node-sass 4.7.2
sass-loader 7.3.1，node-sass 4.14.1
sass-loader 10.0.1，node-sass 6.0.1
