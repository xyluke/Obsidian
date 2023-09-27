# 常识
超出隐藏的css样式
~~~css
.two-line-ellipsis { 
	display: -webkit-box; 
	-webkit-box-orient: vertical; 
	overflow: hidden; 
	text-overflow: ellipsis; 
	-webkit-line-clamp: 2; /* 限制显示两行 */ 
}
~~~
>在上述代码中，要限制显示的内容放在一个带有 `.two-line-ellipsis` 类名的 `div` 元素中。通过设置 `display: -webkit-box;` 和 `-webkit-box-orient: vertical;` 属性，使文本按垂直方向排列。
>
>然后，通过设置 `overflow: hidden;` 和 `text-overflow: ellipsis;` 属性，超出两行的部分将被隐藏，并以省略号代替。
>
>最后，通过设置 `-webkit-line-clamp: 2;` 属性，限制只显示两行文本。
>
>请注意，这种方法目前仅在 WebKit 浏览器中可用，因此需要适配不同浏览器的兼容性，可以使用一些前缀（例如 `-moz-line-clamp`）或考虑使用 JavaScript 实现更复杂的多行文本截断。
>希望这个解决方案对你有所帮助！

# 场景
看到`el-table`中的`el-column`时超出鼠标移动上方会展示详细信息。
`el-form-item`也要超出，两行多余隐藏。想附加展示详情信息的功能。

# 代码实现
实现el-form-item超出隐藏的代码
~~~js
<span v-else slot="label">
	<template>
		<el-tooltip 
			v-if="isLabelOverflow" 
			effect="dark" 
			placement="top-start">
			<div slot="content">{{label}}</div>
			<span class="label-wrapper" ref="labelRef">
				{{label}}
			</span>
		</el-tooltip>
		
		<span v-else class="label-wrapper" ref="labelRef">
			{{label}}
		</span>
	</template>
</span>

data() {
	return {
		isLabelOverflow: false
	}
}

watch: {
	label: {
		handler(nv, old) {
			this.$nextTick(_ => {
				let labelWrapper = '';
				labelWrapper = this.$refs.labelRef;
				this.isLabelOverflow = labelWrapper 
				&& 
				labelWrapper.offsetHeight 
				< labelWrapper.scrollHeight;
			})
		},
		immediate: true // 首次渲染自动触发
	}

},
~~~

# 思路
>1. 设置两个label，`v-if`来控制展示那个。
>2. 监听label字段是否有值，通过`$nextTick`让页面渲染完成时通过`offsetHeigt`（显示的高度）和`scrollHeight`（展平后的高度）来看是否超出文本。

2023-09-27