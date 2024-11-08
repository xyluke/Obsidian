# hydra-form-line-dyn
旧版的form动态的组件容器
里面对hydra-item-select 枚举类特殊处理了
~~~js
// 能编辑的枚举
status: {
	props:{
		value: "status",
		list: null,
		multiple: true,
		type: "hydra-item-select"
	}
}

created() {
	this.$nextTick(() => {
		// ManufactureStatusList 用stringEnumbuild创建的枚举列表
		this.OrderJsonF.status.props.list = this.ManufactureStatusList;
	})
}
~~~

# mk-form-line-dyn
对hydra-item-select 枚举类还原了
~~~js
status: {
	props:{
		value: "status",
		data: null,
		multiple: true,  // 能直接配置了在应用管理，但是mk-form-line-dyn这个组件还是没挂上去
		type: "hydra-item-select"
	}
}
~~~

