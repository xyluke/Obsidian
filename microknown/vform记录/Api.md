> 记录使用到的api

vformRender

| Api名称 | 说明 | 参数 |
| --- | --- | --- |
|setFormJson| 设置vform-json值的| (data)：<br/>data，与Json对应的对象，不是JSON字符串|




widget 容器组件

| Api名称        | 说明             | 参数                                                                               |
| ------------ | -------------- | -------------------------------------------------------------------------------- |
| setTableData | 设置vform-json值的 | (data)：<br/>data，与Json对应的对象，不是JSON字符串                                            |
| getValue     | 获取数据           |                                                                                  |
| setValue     | 设置组件的值         | newValue组件的值                                                                     |
| getWidgetRef | 获取容器或字段组件      | (widgetName, showError)：<br>widgetName，组件名称<br>showError=true/false，如组件不存在是否显示错误 |
| getGlobalDsv | 获取全局数据源变量      |                                                                                  |
| getTableData | 获取tableData的值  |                                                                                  |
