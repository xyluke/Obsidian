> 状态栏

> props
~~~js
    props: {
        value: { type: StringEnum, required: true }, // 对象 状态
        stringEnum: { type: Object, required: true }, // 枚举对象
        label: { type: String, default: "状态" },
        span: { type: Number, default: 6 },
        item: { type: Object },  // 不知道有什么作用
        prop: { type: String, default: "" },
    },
~~~

> 事件
~~~js
// 无事件
~~~


> 使用

~~~js
status: {id: 'Audit', name: '已保存'}
metaForm : [
{
	backType: {id: 'EnumType', name: '枚举'},
	code: 'status',
	dataValue: "",
	disabled: false,
	editTag: true,
	events: undefined,
	id: '6zqTA457e87TXfKHe',
	name: '状态',
	props: {
		value: 'status',
		stringEnum: {
			Saved: {
				id: "Saved",
				name: "已保存",
				tag: "info"
			},
			Audit: {
				id: "Audit",
				name: "已审核",
				tag: "success"
			},
			DeApproval: {
				id: "DeApproval",
				name: "反审核",
				tag: "warning"
			},
			Finish: {
				id: "Finish",
				name: "已完成",
				tag: "warning"
			}
		},
	},
	requiredTag: false,
	self: this,
	type: 'hydra-item-tag-status',
	width: 4
}
]

~~~
