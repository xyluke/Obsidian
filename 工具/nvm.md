# 安装nvm
> node版本管理工具，可以对多个node的版本进行安装和切换 [安装链接](https://github.com/coreybutler/nvm-windows/releases)


![[Pasted image 20230816153607.png]]
[安装步骤链接](http://wed.xjx100.cn/news/225558.html?action=onClick)
链接内容图示
![[node多版本.png]]
# 安装node

```text
1.nvm list avaliable 
显示可以下载版本的部分列表

2.nvm install 版本号 (nvm install 14.15.4) 
安装指定版本(LTS: 长期稳定版本)

3.nvm list
查看当前下载的版本, *表示当前使用的版本

4. nvm use  版本号 (nvm use 14.15.4) 
指定使用当前node的版本 (一般需要在管理员权限下操作此命令)

5.nvm uninstall 版本号(nvvm uninstall 14.15.4)
即可以删除对应的版本

6. where node 
查看node.exe的安装目录
```

# 报错解决
## node 安装后没有npm解决办法
1.下载

> npm下载地址：[npm.taobao.org/mirrors/npm…](http://npm.taobao.org/mirrors/npm/ "http://npm.taobao.org/mirrors/npm/") （下载对应版本的zip文件）  
> node版本对应npm版本：[nodejs.org/zh-cn/downl…](https://nodejs.org/zh-cn/download/releases/ "https://nodejs.org/zh-cn/download/releases/")

2.nvm 对应node版本目录 node_modules下面 改名字为npm![](https://img-blog.csdnimg.cn/b58ad949384744e8808a27d8ecc23b1a.png)

3.npm 去npm的bin目录下面npm npm.cmd 复制![](https://img-blog.csdnimg.cn/1b301ea82f0f4b7fbca2c01d25bf739a.png)

4.npm -v 查看是否安装成功
![](https://img-blog.csdnimg.cn/b125fa7f1e124dd7bba716213dfc6eb9.png)



## 配置环境变量
~~~js
需要配置环境变量
我的电脑-->属性-->高级系统配置-->环境变量配置 
NVM_HOME: nvm.exe所在的文件夹路径 
NVM_SYMLINK: 安装时设置的node_nvm快捷方式的路径 配置上就可以了
~~~
# nvm-setting.txt文件

~~~js
root: C: vmdev vm //nvm.exe 所在路径 

path: C: odejs_nvm //node的快捷方式存放路径

node_mirror: http://npm.taobao.org/mirrors/node/ //node 镜像 

npm_mirror: https://npm.taobao.org/mirrors/npm/ //npm 镜像

proxy: //代理 可不填 
arch: 64 //操作系统位数 
~~~
