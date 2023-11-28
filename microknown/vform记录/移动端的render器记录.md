
# subform的disabled
subform的disabled功能
> 在taro中subform组件他的disabled实现。rowRef记录的是当前的subform所有的组件。直接在此处调用组件内部的setDisabled方法是否就能实现disabled的功能呢？


> rowRef是当前subform的管理的widgetList。这里有两个rowRef 就是两个subform大组件。第二个是有两行。
![[../../img/Pasted image 20231127095723.png]]


# 容器的校验问题
> 初步打算是把容器的根据widgetList内部的 一个标识字段 `formItemFlag` 来表示。全局表单数据获取。然后将`required`的为true的字段获取出来看值是否为空。

~~~js
filterFormModel = (widgetLists, requires:any=[], cardLabel='') => {
	// requires 是全部的widgetList 深拷贝一份  cardLabel 是因为subform中没有label值所以手动填写一份
	let requireArray = requires;
	let widget = JSON.parse(JSON.stringify(widgetLists));
	widget.forEach(item => {
		// 1. 容器
		if (item.category === 'container') {
			if (item.type === 'card') {
				let length = requireArray.length;
				if (item.options.labelHidden) {
					requireArray[length] = {
						name: item.options.name, 
						label: '', 
						children: []
					}
				} else {
					requireArray[length] = {
						name: item.options.name, 
						label: item.options.label, 
						children: []
					}
				}
				this.filterFormModel(item.widgetList, requireArray[length].children, item.options.label);
			} else if(item.type === 'grid') {
				this.filterFormModel(item.cols, requireArray);
			} else if(item.type === 'sub-form'){
				let length = requireArray.length;
				requireArray[length] = {
					name: item.options.name, 
					label: cardLabel, 
					children: [], 
					subform: true
				}
				this.filterFormModel(item.widgetList, requireArray[length].children)
			} else {
				this.filterFormModel(item.widgetList, requireArray);
			}
		} else {
			// 此处必填 往requireArray中给值
			if (item.formItemFlag && item.options.required) {
				requireArray.push({
				name: item.options.name, 
				label: item.options.label
				})
			}
		}
	})

	return requireArray;
}
~~~

> 用户校验动态表单的必填项方法
~~~JS
validataFormData = () => {
	// 获取formData的数据
	let data = this.getFormData();
	// requireItems的值是必填项的值。 定义在类组件的同级。这样不会造成死循环
	return this.iterativeRequire(requireItems, data);
}
~~~
`requireItems`
![[../../img/Pasted image 20231128162659.png]]
![[../../img/Pasted image 20231128162808.png]]

> 遍历必填项
~~~js
iterativeRequire = (requireItems, data) => {
	let flag = requireItems.some(item => {
		if (item.children && item.children.length > 0) {
			if (item.subform) {
				return this.checkRequire(item, data, true);
			} else {
				return !this.iterativeRequire(item.children, data);
			}
		} else {
			return this.checkRequire(item, data, false);
		}
	})
	return !flag;
}
~~~

> 检查必填项
~~~js
checkRequire = (checkItem, data, subform=false) => {
	if (subform) {
		let checkItemName = checkItem.name;
		let subformRequireList = checkItem.children;
		let value = data[checkItemName];
		let flag = false;
		for (let i = 0; i < value.length; i++) {
			if (flag) {
				return flag;
			}
			for(let j = 0; j < subformRequireList.length; j++) {
				let key = subformRequireList[j].name;
				let label = subformRequireList[j].label;
				let tempV = value[i][key];
				if (tempV === undefined || 
				tempV === null || 
				tempV === '' || 
				Array.isArray(tempV) && !tempV.length
				) {
					this.totalAction(`【${checkItem.label}】序号${i+1}【${label}】 未填写值`);
					flag = true;
					return true;
				}
			}
		}
	} else {
		let value = data[checkItem.name];
		let label = checkItem.label;
		if (value === undefined || 
		value === null || 
		value === '' || 
		Array.isArray(value) && !value.length) {
			this.totalAction(`【${label}】 未填写值`);
			console.log(1.1);
			return true;
		}
	}
	return false
}

// 售后更改 当card的children为空的时候还是会校验
if (data.hasOwnProperty(checkItem.name)) {
	let value = data[checkItem.name];
	let label = checkItem.label;
	if (value === undefined || 
	value === null || 
	value === '' || 
	Array.isArray(value) && !value.length && !checkItem.subform) {
		this.totalAction(`【${label}】 未填写值`);
		return true;
	}
} else {
	return false
}
~~~


