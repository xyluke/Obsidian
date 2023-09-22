> 选择框

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
        span: { type: Number, default: 6},
        clearable: { type: Boolean, default: false}, // 清除按钮
    },
~~~

> 事件
~~~js
// update: 更新事件
update(val) {
	this.$emit("input", val);
}

// change: 改变事件
change(val) {
	let ret = null;
	if (this.multiple === false) {
		ret = list.find2(this.data, item => item.id === val);
	} else {
		ret = [];
		for (let v of val) {
			let t = list.find2(this.data, item => item.id === v);
			if (t !== null) {
				ret.push(t);
			}
		}
	}
	this.$emit("change", val);
	this.$emit("change-item", ret);
}


// 单选：v是字符串
// 多选：v是字符串数组
calcItemName(v) {
	this.itemName = "";
	let as = [];
	if (this.multiple === false) {
		if (!common.strIsEmpty(v)) {
			as.push(v);
		}
	} else {
		as = v;
	}
	for (let a of as) {
		let it = list.find2(this.data, item => item.id === a);
		if (it !== null) {
			if (common.strIsEmpty(this.itemName)) {
				this.itemName = it.name;
			} else {
				this.itemName = this.itemName + ", " + it.name;
			}
		}
	}
	this.$emit('selectEvent', v);
}
~~~


> 使用

~~~js
metaForm : [
	{
		backType: {id: 'EnumType', name: '枚举类'},
		code: 'select',   // 单选 字符串 多选 []
		dataValue: "",
		disabled: false,
		editTag: true,
		events: {
			input: 'yourInput',
			change: 'yourChange',
			'change-item': 'yourChangeItem',
			selectEvent: 'selectEvent',
		},
		id: '6pYhoO9EHgNFiD8U',
		name: '选择组件',
		props: {
			data: [
			{id: 1, name: 'select1'}, 
			{id: 2, name: 'select2'},
			{id: 3, name: 'select3'}],
			multiple: true
		},
		requiredTag: false,
		self: this,
		type: 'hydra-item-select',
		width: 12
	}
]

methods : {
	yourChange(val) { console.log(val)};
	yourChange(val) { console.log(val)};
	yourChangeItem(ret) {console.log(ret)}; // 选择的对象值
	selectEvent(val) {console.log(val)} // 自定义的事件，自己扩展
}
~~~
