
# subform的disabled
subform的disabled功能
> 在taro中subform组件他的disabled实现。rowRef记录的是当前的subform所有的组件。直接在此处调用组件内部的setDisabled方法是否就能实现disabled的功能呢？


> rowRef是当前subform的管理的widgetList。这里有两个rowRef 就是两个subform大组件。第二个是有两行。
![[../../img/Pasted image 20231127095723.png]]


# 容器的校验问题
> 初步打算是把容器的根据widgetList内部的 一个标识字段 `formItemFlag` 来表示。全局表单数据获取。然后将`required`的为true的字段获取出来看值是否为空。

~~~js
filterFormModel = (widgetLists, requires:any=[], cardLabel='') => {
        let requireArray = requires;
        let widget = JSON.parse(JSON.stringify(widgetLists));
        widget.forEach(item => {
            // 1. 容器
            if (item.category === 'container') {
                if (item.type === 'card') {
                    let length = requireArray.length;
                    if (item.options.labelHidden) {
                        requireArray[length] = {name: item.options.name, label: '', children: []}
                    } else {
                        requireArray[length] = {name: item.options.name, label: item.options.label, children: []}
                    }
                    this.filterFormModel(item.widgetList, requireArray[length].children, item.options.label);
                } else if(item.type === 'grid') {
                    this.filterFormModel(item.cols, requireArray);
                } else if(item.type === 'sub-form'){
                    let length = requireArray.length;
                    requireArray[length] = {name: item.options.name, label: cardLabel, children: [], subform: true}
                    this.filterFormModel(item.widgetList, requireArray[length].children)
                } else {
                    this.filterFormModel(item.widgetList, requireArray);
                }
            } else {
                // 此处必填
                if (item.formItemFlag && item.options.required) {
                    requireArray.push({name: item.options.name, label: item.options.label})
                }
            }
        })


        return requireArray;
    }
~~~
