# vform数据源
![[../img/Pasted image 20240717154330.png]]

唯一名称建议用英文

请求地址
* 公司内部使用到hydra-web上面的话就是 `meta.MetaDynamic/summary`就好了
* ![[../img/Pasted image 20240717154651.png]]
* 外部的公司就完整地址，但是现在并未对外开放


# 参数与模板
![[../img/Pasted image 20240717155331.png]]

![[../img/Pasted image 20240717155118.png]]

![[../img/Pasted image 20240717155622.png]]


# vform的多个tab切换数据问题
当多个数据切换的时候不同的参数因为是详情页面的关系。所以四个不同的板块参数要特殊的给值。用之前的预处理方法给this上挂在不同板块接口数据源的参数。

`flow.FlowableTask/myProcess`
![[../img/Pasted image 20240717165755.png]]

`flow.FlowableTask/todoList`
![[../img/Pasted image 20240717165838.png]]