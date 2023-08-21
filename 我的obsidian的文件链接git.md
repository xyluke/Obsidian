今早上我使用obsidian把文件给传递到了github上面


# 问题
1. 有时候Obsidian Git 插件并不能够安装成功，或许是网络问题。但是，可以直接在github上下载三个文件进行拷贝下来。

2. 只想在单个文件夹仓库绑定对应的github的仓库

git init  // 初始化当前仓库  与.obsidian 同级

git  config user.name '你的账户名'
git config user.email '你的邮箱名'

cat  config // 查看你的的配置文件
![[img/Pasted image 20230821110638.png]]
`credential 是为了能够自动记住密码来设置的`
git config credential.helper store  // 创建证明来让之后的不需要密码就可以连接仓库
![[img/Pasted image 20230821111026.png]]
![[img/Pasted image 20230821112638.png]]
`git push的时候报错`
git config --global http.sslVerify false 
~~~text
fatal: unable to access 'https://github.com/xy/Obsidian.git/': SSL certifica te problem: unable to get local issuer certificate
// git push时候的报错
这是因为本地的ssl证书无法对应 所以我们在这个文件里面进行忽略
git config --global http.sslVerify false  // 这个命令来关闭ssl的检测
~~~
![[img/Pasted image 20230821112854.png]]
git remote set origin 'git 仓库的地址'
git checkout -b main  // 建立main的分支

git add .
git commit -m "init: 第一次初始化obsidian的代码"
git push  --set-upstream origin main  // 链接远程仓库的分支

# .gitignore 处理之前上传过的文件

![[img/Pasted image 20230821113359.png]]

![[img/Pasted image 20230821113418.png]]