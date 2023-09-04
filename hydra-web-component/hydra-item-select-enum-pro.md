## 枚举组件

> 枚举选项时使用

metaJson
~~~js
{
	backType: {id: 'String', name: '字符串'},
	code: 'enumcodeMul',
	dataValue: "",
	disabled: false,
	editTag: true,
	events: {},
	id: '6pYhoO9EHgNFiD87779',
	name: '枚举类型',
	props: {
		span: 12,
		data: [
		{id: '1', name: '选项枚举一'}, 
		{id: '2', name: '选项枚举二', disabled: true}, 
		{id: '3', name: '选项枚举三'},
		{id: '4', name: '选项枚举四'},
		{id: '5', name: '选项枚举五'}], // data 选项列表值
		multiple: true
	},
	requiredTag: false,
	self: this,
	type: 'hydra-item-select-enum-pro',
	width: 8
}
~~~