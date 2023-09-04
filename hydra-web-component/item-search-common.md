> 搜索通用组件

> props
~~~js
    props: {
		// 搜索对象值
		// "mdm-material" "mdm-material2" 
		// "mdm-customer" "mdm-supplier"
		// "sale-order" "purchase-order"
        dataValue: {
            type: String,
            required: true
        },
        label: {
            type: String,
            default: "",
        },
        value: {
            type: String,
            require: true
        },
        prop: {
            type: String,
            default: ""
        },
        display: {
            type: String,
            default: ""
        },
        disabled: {
            type: Boolean,
            default: false
        },
        span: {
            type: [String, Number],
            default: 6
        },
        // @todo 暂时不支持多选
        multi: {
            type: Boolean,
            default: false
        },
        statusList: {
            type: Array,
            default: () => []
        },
        showOp: {
            type: Boolean,
            default: true
        },
        // 动态表单的元数据
        meta: {
            type: Object
        },
        vm: {
            type: Object
        }
    },
~~~

> 事件
~~~js
// input: 输入事件
input(value) {
	this.value = value;
	this.$emit('input', value);
}

// change: 改变事件
change(val) {
	this.value = this.value instanceof Object ? val : val.id;
	this.$attrs.displayCode && this.changeDisplayCode(val);
	this.$emit("change", val);
}
~~~


> 使用

~~~js
metaForm : [
	{
		backType: {id: 'String', name: '字符串'},
		code: 'material.id',
		dataValue: "mdm-material2",
		disabled: false,
		editTag: true,
		events: {
			change: 'changeMaterial',
			input: 'inputMaterial'
		},
		id: '6pYhoO9EHgNFiD8U',
		name: '物料',
		props: {
			span: 12,
			display: 'material.name',
		},
		requiredTag: false,
		self: this,
		type: 'hydra-item-search-common',
		width: 8
	}
]

methods : {
	changeMaterial(val) { console.log(val)};
	inputMaterial(val) { console.log(val)};
}
// 搜索对象值
// "mdm-material" "mdm-material2" 
// "mdm-customer" "mdm-supplier"
// "sale-order" "purchase-order"
~~~
