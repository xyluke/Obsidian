> 选择框 code和name

> props
~~~js
    props: {
		data: { type: Array, default: () => [], required: true },
	    value: { type: String, default: ""},
	    // prop 参数用于必填 双绑定
	    prop: { type: String, default: ""}, 
        disabled: { type: Boolean, default: false},
        required: { type: Boolean, default: false},
        label: { type: String, default: ""},
        span: { type: Number, default: 6}
    },
~~~

> 事件
~~~js
// update: 更新事件
update(val) {
	this.$emit("input", val);
},

// change: 改变事件
change(val) {
	this.$emit("change", change);
}
~~~


> 使用

~~~js
codePlus: []
metaForm : [
	{
		backType: {id: 'OrderBy', name: '按照order'},
		code: 'codePlus',   // 单选 字符串 多选 []
		dataValue: "",
		disabled: false,
		editTag: true,
		events: {
			input: 'yourInput',
			change: 'yourChange',
		},
		id: '6pYhoO9EHgNFiD8U',
		name: 'code选择',
		props: {
			data: [
			{code: 1, name: 'select1'}, 
			{code: 2, name: 'select2'},
			{code: 3, name: 'select3'}],
			multiple: true
		},
		requiredTag: false,
		self: this,
		type: 'hydra-item-select-code',
		width: 12
	}
]

methods : {
	yourInput(val) { console.log(val)};
	yourChange(val) { console.log(val)};
}
~~~
