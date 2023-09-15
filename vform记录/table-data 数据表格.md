9-15
seetting-panel 
| -property-editor
| --container-data-table
| ---date-table-customClass-editor 

添加fieldTypeOptions
~~~js
fieldTypeOptions: [
	{
		id: 'TEXT',
		name: '文本'
	}, {
		id: 'DIGIT',
		name: '数字'
	}, {
		id: 'DATETIME',
		name: '日期'
	},{
		id: 'BOOLEAN',
		name: '布尔'
	}, {
		id: 'QUOTE',
		name: '引用'
	}, {
		id: 'DICT',
		name: '字典'
	}, {
		value: 'PICTURE',
		name: '图片'
	}
],
~~~