> 数字区间范围


> props
~~~js
props: {
	label: { type: String, default: "数字区间" },
	span: { type: Number, default: 6 },
	value: { type: Object, default: () => ['', ''] },
	prop: { type: String, default: "" },
	disabled: { type: Boolean, default: false}
}
~~~

> 事件
~~~js
// 更新事件
check() {
	this.debounce(()=> {
		let [pre, last] = this.numberRange;
		if(pre && last && new BigNumber(pre).gt(last)) {
		   this.$message.warning("区间不正确");
		   this.numberRange[1] = "";
		   this.$refs.maxs.value = "";
		   this.$emit("input", this.numberRange)
		}
	}, 1000)
}
// 此处暴露出 input 1个方法
~~~


> 使用

~~~js
metaForm : [
	{
		backType: {id: 'PriceRanges', name: '金额范围'},
		code: 'totalPriceRanges',
		dataValue: "",
		disabled: false,
		editTag: true,
		events: {
			input: 'numberInput'
		},
		id: '6pYhoO9EHgNFiD8U',
		name: '数字区间范围', //对应组件的label
		props: {
			span: 12
		},
		requiredTag: false,
		self: this,
		type: 'hydra-item-range-number',
		width: 8
	}
]

methods : {
	numberInput(val) { console.log(val)};
}

~~~
