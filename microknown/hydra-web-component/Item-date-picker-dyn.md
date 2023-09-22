> 当前时间范围

> props
~~~js
props: {
	disabled: { type: Boolean, default: false, },
	// 必填的参数字符串
	prop: { type: String, default: "" },
	// 值
	value: { type: String, default: "" },
	// 展示字段
	label: { type: String, default: "", },
	// 提示选择文字
	holder: { type: String, default: "", },
	// 宽度
	span: { type: Number, default: 6 },
}
~~~

> 事件
~~~js
// 更新事件
update(val) {
	this.$emit("input", val);
},


change(val) {
	this.$emit("change", val);
	this.$emit("input", val);
}
// 此处暴露出 input  change 两个方法
~~~


> 使用

~~~js
metaForm : [
	{
		backType: {id: '', name: '时间'},
		code: 'times',
		dataValue: "",
		disabled: false,
		editTag: true,
		events: {
			change: 'datePickerChange',
			input: 'datePickerInput'
		},
		id: '6pYhoO9EHgNFiD8U',
		name: '时间time范围', //对应组件的label
		props: {
			span: 12,
			holder: '选择当前的时间'
		},
		requiredTag: false,
		self: this,
		type: 'hydra-item-date-range-picker-dyn',
		width: 8
	}
]

methods : {
	datePickerChange(val) { console.log(val)};
	datePickerInput(val) { console.log(val)};
}

~~~
