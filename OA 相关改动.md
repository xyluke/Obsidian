8-15
注释掉了getByForm接口。元数据现在还不需要。
修改表单更改时版本更改失败的问题。现象：浏览器端改后变成移动端
所有任务添加active事件
退出问题和图片预览问题

![[img/Pasted image 20230829094027.png]]


8-29

![[img/Pasted image 20230829143312.png]]
元素类型无效:期望是字符串(对于内置组件)或类/函数(对于复合组件)，但得到:未定义。
原因是使用了RichText，不知道这个版本可能不太支持的原因
最后使用了一个新的方法
> React 的 `dangerouslySetInnerHTML`

![[img/Pasted image 20230829143631.png]]

