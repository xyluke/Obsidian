> 时间范围

> props
~~~js
    props: {
	    value: { type: Array, default: ["", ""]},
	    // prop 参数用于必填 双绑定
	    prop: { type: String, default: ""}, 
        disabled: { type: Boolean, default: false},
        required: { type: Boolean, default: false},
        label: { type: String, default: ""},
        span: { type: Number, default: 6},

        // placeholder 提示输入信息
        holder: { type: String, default: ""},

        // meta对应当前组件的值
        meta: { type: Object},
        vm: { type: Object} //组件总数居
    },
~~~

> 事件
~~~js
// 更新事件
update(val) {
	let rs = [];
	if (val != null) {
		const startStr = datetime.formatToDate(val[0]);
		const endStr = datetime.formatToDate(val[1]);
		rs = [startStr, endStr];
	}
	this.$emit("input", rs);
	this.$emit("change", rs);
}

// 此处暴露出 input  change 两个方法
~~~


> 使用

~~~js
metaForm : [
	{
		backType: {id: 'DateTime', name: '日期'},
		code: 'dateRanges',
		dataValue: "",
		disabled: false,
		editTag: true,
		events: {
			change: 'dateRangeChange',
			input: 'dateRangeInput'
		},
		id: '6pYhoO9EHgNFiD8U',
		name: '时间范围',
		props: {
			span: 12
		},
		requiredTag: false,
		self: this,
		type: 'hydra-item-date-range-picker-dyn',
		width: 8
	}
]

methods : {
	dateRangeChange(val) { console.log(val)};
	dateRangeInput(val) { console.log(val)};
}

~~~
