> 字典类型选择

> props
~~~js
    props: {
		dataValue: {type: String, required: true},
		multiple: { type:},
	    value: { type: String, default: ""},
	    // prop 参数用于必填 双绑定
	    prop: { type: String, default: ""}, 
        disabled: { type: Boolean, default: false},
        required: { type: Boolean, default: false},
        label: { type: String, default: "字典单选"},
        span: { type: Number, default: 6}
    },
~~~

> 事件
~~~js
update(val) {
	let d = this.find(val);
	if (d == null) {
		this.$emit("input", null, this.prop);
		return;
	}
	this.$emit("input", val, this.prop);
},

change(val) {
	this.$emit("change", val);
},

onClear() {
	this.itemName = "";
	this.valueDict = "";
	this.$emit("input", null, this.prop);
	this.$emit("change", null);
},
~~~


> 使用

~~~js
metaForm : [
{
	backType: {id: '', name: ''},
	code: 'dictCode',
	dataValue: "",
	disabled: false,
	editTag: true,
	events: {
		input: 'yourInput',
		change: 'yourChange'
	},
	id: '6xeVNxJ645kEWvS4',
	name: '字典',
	props: {
		dataValue: 'HrProject'          
	},
	requiredTag: false,
	self: this,
	type: 'hydra-item-select-dict-type-dyn',
	width: 12
},
]

methods : {
	yourInput(val) { console.log(val)};
	yourChange(val) { console.log(val)};
}
~~~
