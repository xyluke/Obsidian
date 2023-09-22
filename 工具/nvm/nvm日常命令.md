## 查看当前nvm可安装node的版本
`nvm list available`
![[../../img/Pasted image 20230922103304.png]]
## 当前安装的版本信息
`nvm version`
![[../../img/Pasted image 20230922103400.png]]

## 查看当前安装的node
`nvm ls`
![[../../img/Pasted image 20230922112423.png]]

## 切换指定的node
`nvm use ` 版本号
![[../../img/Pasted image 20230922103525.png]]
> 乱码则需要以`管理员身份`开启的`命令行`
![[../../img/Pasted image 20230922103653.png]]

> 解决方法2
> 1. 控制台输入 chcp 65001  
> 2. 输入 nvm use 14.15.4
> 3. 看提示信息，发现是

![[../../img/Pasted image 20230922110132.png]]
>意思: 您没有足够的权限来执行此操作 （你的权限不够）
>用`管理员身份`打开`命令行`输入命令就好


## 删除安装的版本
`nvm uninstall` 版本号



## 设置默认的版本
~~`nvm alias default 11.1.0`~~
>不能使用

