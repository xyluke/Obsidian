> version2.0
> 表单动态

> 解决的问题
1. props 的扩展字段能够自动添加到组件上面，不用手动去改组件库代码。
2. events属性能够更好的绑定方法，现在可以直接指定 change input  或者其他方法。
3. disable控制逻辑混乱的问题改动。让disabled可以自控，初始化不影响源数据。
4. required必填属性中rules的自定义和系统生成。

`vue代码`

~~~js
<template>
	<el-form 
	:label-width="inLabelWidth" 
	size="small" 
	:rules="formRules" 
	ref="formRef" 
	:disabled="disabled"
	:model="model">
	    <el-row :gutter="gutter">
	        <component
	            v-for="componentItem in meta"
	            :key="componentItem.id"
	            :is="componentItem.type"
	            v-bind="transferComponent(componentItem)"
	            v-on="getEventHandlers(componentItem)">
	        </component>
	        <slot name="foot"></slot>
	    </el-row>
	</el-form>
</template>
~~~

`核心方法`
~~~js
// 转化基础数据字段
transferComponent(componentItem) {
            const {
                width: span,
                code, name: label,
                disabled, editTag,
                requiredTag,
                self,
                events,
                props

            } = componentItem;
            return {
                label,
                span,
                disabled,
                editTag,
                requiredTag,
                code,
                prop: code,
                self,
                meta: componentItem,
                vm: this.model,
                events,
                ...props,
                
                display: 
                props.display ? 
                props.display.split('.').
                reduce((a,b) => a[b], this.model) : '',
                
                value: code && 
                code.split('.').reduce((a, b) => a[b], this.model),
            }
}，

// 将componentItem的events 方法注册上去  events: {change: 'changeMy'}
// self 是当前组件的this
getEventHandlers(componentItem) {
const eventHandlers = {};
// 添加input基础事件  双向绑定
eventHandlers['input'] = (val) => {this.inputOrigin(val, componentItem.code)};
 
if (componentItem && componentItem.events) {
   for (let key in componentItem.events) {
        let functionName = componentItem.events[key];
        // 将self目标组件的方法注册上去
        eventHandlers[key] = componentItem.self[functionName];
                }
  }
  return eventHandlers;
},
~~~