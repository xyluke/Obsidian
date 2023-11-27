
# subform的disabled
subform的disabled功能
> 在taro中subform组件他的disabled实现。rowRef记录的是当前的subform所有的组件。直接在此处调用组件内部的setDisabled方法是否就能实现disabled的功能呢？


> rowRef是当前subform的管理的widgetList。这里有两个rowRef 就是两个subform大组件。第二个是有两行。
![[../../img/Pasted image 20231127095723.png]]


# 容器的校验问题
> 初步打算是把容器的根据widgetList内部的 一个标识字段 `formItemFlag` 来表示。全局表单数据获取。然后将`required`的为true的字段获取出来看值是否为空。