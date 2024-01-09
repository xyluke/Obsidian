> vform的链式调用，为了满足如material.name.id这种形式的数据

formRender中的构建表单的widget列表组件方法，添加了新的方法`buildDataFromWidget`中新添加方法如下：

![[../../img/Pasted image 20240109153041.png]]


#  fieldMixin.js 中新添加了方法

![[../../img/Pasted image 20240109154343.png]]
> 用来将更新的值赋值给formModel，subFormDataRow中也能良好的进行赋值


#  initFieldModel 中添加了selfChainMk方法

![[../../img/Pasted image 20240109154740.png]]

![[../../img/Pasted image 20240109154823.png]]

![[../../img/Pasted image 20240109154929.png]]
