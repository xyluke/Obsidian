> 输入框

> props
~~~js
    props: {
	    value: { type: String, default: ""},
	    // prop 参数用于必填 双绑定
	    prop: { type: String, default: ""}, 
        disabled: { type: Boolean, default: false},
        required: { type: Boolean, default: false},
        label: { type: String, default: ""},
        span: { type: Number, default: 6},

		// number是数字 string字符串
        type: { type: String, default: "String"},
        // 插入到输入框后面的代码
        append: { type: String, default: ""},
        // placeholder 提示输入信息
        holder: { type: String, default: ""},
        // 输入行数 只对type为textarea有作用
        rows: { type: Number, default: 1 },
		// 最大输入长度
        maxLength: { type: Number, default: 64 },
        // 是否校验数字
        exam: { type: Boolean, default: false},

        // meta对应当前组件的值
        meta: { type: Object}
    },
~~~

> 事件
~~~js
// update: 更新事件
update(val) {
	this.$emit("input", val);
	if (this.$listeners && this.$listeners.change) {
		this.$emit('change', val);
	}
}

// 清除事件
remove() {
	this.$emit('input', '');
	this.$emit('clear');
}
~~~


> 使用

~~~js
metaForm : [
	{
		backType: {id: 'String', name: '字符串'},
		code: 'code',
		dataValue: "",
		disabled: false,
		editTag: true,
		events: {
			clear: 'clearEvent',
			input: 'myInput'
		},
		id: '6pYhoO9EHgNFiD8U',
		name: '销售单号',
		props: {
			span: 12
		},
		requiredTag: false,
		self: this,
		type: 'hydra-item-input-dyn',
		width: 8
	}
]

methods : {
	myInput(val) { console.log(val)};
	clearEvent() { console.log('清除后执行的事件')};
}

// 如果是number类型则只需要在props中添加
props: {
	type: 'number',
	exam: true
}
~~~
