# vform的基本信息
[vform官网地址](https://vform666.com/)
[开发文档](https://www.yuque.com/visualdev/vform/)
[Gitee地址](https://gitee.com/vdpadmin/VFormBuilds)
[Github地址](https://github.com/vform666/variant-form)

# 使用VFormDesigner

## 1. 引入并全局注册VForm组件
~~~js
import Vue from 'vue'
import App from './App.vue'
import axios from 'axios'  //如果需要axios，请引入

import ElementUI from 'element-ui'  //引入element-ui库
import VForm from 'vform-builds'  //引入VForm库

import 'element-ui/lib/theme-chalk/index.css'  //引入element-ui样式
import 'vform-builds/dist/VFormDesigner.css'  //引入VForm样式

Vue.config.productionTip = false

Vue.use(ElementUI)  //全局注册element-ui
Vue.use(VForm)  //全局注册VForm(同时注册了v-form-designe、v-form-render等组件)
/* 注意：如果你的项目中有使用axios，请用下面一行代码将全局axios复位为你的axios！！ */
window.axios = axios

new Vue({
  render: h => h(App),
}).$mount('#app')
~~~


## 2. 在Vue的模板中使用组件

*文档中的使用*
~~~js
<template>
  <v-form-designer></v-form-designer>
</template>

<script>
  export default {
    data() {
      return {
      }
    }
  }
</script>

<style lang="scss">
body {
  margin: 0;  /* 如果页面出现垂直滚动条，则加入此行CSS以消除之 */
}

</style>
~~~

*实际项目中使用*
> 局部注册VFormDesigner

~~~js
<v-form-designer 
	ref="vfDesigner" 
	:designer-config="designerConfig" 
	:globalDsv="globalDsv"
/>


<script>
// import { VFormDesigner } from "vform-builds-pro"; 自己改造后的库
import { VFormDesigner } from "vform-builds";

export default {
	components: {
		VFormDesigner
	},
	data() {
		return {
			// 设计器的基础配置信息高度 功能键隐藏等
			designerConfig: {
                logoHeader: false,
                toolbarMaxWidth: 420,  //设计器工具按钮栏最大宽度（单位像素）
                toolbarMinWidth: 300,  //设计器工具按钮栏最小宽度（单位像素）
                defaultURL: defaultURL,
	            exportCodeButton: false,
                generateSFCButton: false,
                // defaultURL: "http://192.168.2.2:8011/api/portal.File/upload2",
                // toolbarMaxWidth: 510
                formTemplates: false
            },
            // 预留的vform全局的数据
	        globalDsv: this, // 我把vm的this传入内部好使用这里的方法和变量
		}
	}

}
</script>
~~~


# 使用VFormRender
如果之前全局注册了VFormDesigner组件，则不再需要注册VFormRender组件。仅当在项目中独立使用VFormRender组件（即项目中不含表单设计器）时才需要注册。

## 1. 引用且全局注册
*文档使用*  
~~~js
import Vue from 'vue'
import App from './App.vue'
import axios from 'axios'  //如果需要axios，请引入

import ElementUI from 'element-ui'  //引入element-ui库
import VFormRender from 'vform-builds/dist/VFormRender.umd.min.js'  //引入VFormRender组件

import 'element-ui/lib/theme-chalk/index.css'  //引入element-ui样式
import 'vform-builds/dist/VFormRender.css'  //引入VFormRender样式

Vue.config.productionTip = false

Vue.use(ElementUI)  //全局注册element-ui
Vue.use(VFormRender)  //全局注册VFormRender等组件
/* 注意：如果你的项目中有使用axios，请用下面一行代码将全局axios复位为你的axios！！ */
window.axios = axios

new Vue({
  render: h => h(App),
}).$mount('#app')
~~~

## 2. 在Vue的模板中使用组件
*文档中使用*
~~~js
<template>
  <div>
    <v-form-render 
    :form-json="formJson" 
    :form-data="formData" 
    :option-data="optionData" 
    ref="vFormRef"
    >
    </v-form-render>
    <el-button type="primary" @click="submitForm">Submit</el-button>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        /* 注意：formJson是指表单设计器导出的json，
        此处演示的formJson只是一个空白表单json！！ */
        formJson: {
			"widgetList":[],
			"formConfig":
			{
				"labelWidth":80,
				"labelPosition":"left",
				"size":"",
				"labelAlign":"label-left-align",
				"cssCode":"",
				"customClass":"",
				"functions":"",
				"layoutType":"PC",
				"onFormCreated":"",
				"onFormMounted":"",
				"onFormDataChange":""
			}
        },
        formData: {},
        optionData: {}
      }
    },
    methods: {
      submitForm() {
        this.$refs.vFormRef.getFormData().then(formData => {
          // Form Validation OK
          alert( JSON.stringify(formData) )
        }).catch(error => {
          // Form Validation failed
          this.$message.error(error)
        })
      }
    }
  }
</script>
~~~


*实际项目使用*
~~~js
<v-form-render 
:form-json="formConf" 
:form-data="variablesData" :
key="new Date().getTime()" 
ref="vFormRef" 
:globalDsv="globalDsv"
/>

~~~

# 容器和字段组件
## 容器
![[../../../img/Pasted image 20240125113524.png]]

## 字段组件
![[../../../img/Pasted image 20240125113558.png]]

# 属性（prop）
## Designer中的属性

![[../../../img/Pasted image 20240125112729.png]]


## Render中的属性

![[../../../img/Pasted image 20240125112848.png]]


# API

## Designer中使用的API
*项目中常用的API*

| Api名称 | 说明 | 参数 |  |
| ---- | ---- | ---- | ---- |
| getFormJson | 获取vform-json值的 |  | 获取的是Json字符串 |
| setFormJson | 设置vform-json值的 | (data)：<br/>data，与Json对应的对象，不是JSON字符串 | 设计器中用来把获取到的json字符串转换的对象，还原为页面 |
| clearDesigner | 清空设计器画布 |  | 重新进入要清除掉画布 |

~~~js
// getFormJson
let formJson = this.$refs.vfDesigner.getFormJson();
this.form.formContent = JSON.stringify(formJson);
this.submitForm();

// setFormJson
const formData = JSON.parse(data.formContent); // 导出的json字符串格式
this.$refs.vfDesigner.setFormJson(formData);
~~~

*所有的设计器API方法*
![[../../../img/Pasted image 20240125111612.png]]


## Render中使用的API
*项目中常用的API*

| Api名称 | 说明 | 参数 | 补充 |
| ---- | ---- | ---- | ---- |
| getFormData | 获取表单的数据 |  | 获取当前render中用户填写的数据。 |
| setFormData | 设置表单的数据 | formData，表单数据JSON对象 | 注意是JSON对象，不是字符串 |
| disableForm | 禁用表单编辑 |  | 用于审核不可编辑的业务 |
| enableForm | 启用表单编辑 |  | 用于普通反审核可以编辑的业务 |
| hideWidgets | 隐藏一个或多个组件 | widgetNames，组件名称，格式为字符串或字符串数组 |  |
| showWidgets | 显示一个或多个组件 | 显示一个或多个组件	widgetNames，组件名称，格式为字符串或字符串数组 |  |
| getWidgetRef | 根据code来获取容器或字段组件的vm实体 | （widgetName，showError）:组件名称，如果组件不存在展示的错误信息 | 获取的是Json字符串 |
| getGlobalDsv | 获取全局数据源变量 |  |  |
| validateForm | 表单数据是否验证通过 | 表单数据是否验证通过	callback(valid)，回调函数，接受valid参数（true/false） |  |

*所有的渲染器API方法*

![[../../../img/Pasted image 20240125110657.png]]


## 容器中使用的API
| Api名称 | 说明 | 参数 | 补充 |
| ---- | ---- | ---- | ---- |
| getWidgetRef | 根据code来获取容器或字段组件的vm实体 | （widgetName，showError）:组件名称，如果组件不存在展示的错误信息 | 获取的是Json字符串 |
*所有的容器API方法*

![[../../../img/Pasted image 20240125111801.png]]


## 字段组件中使用的API

*全部字段的API方法*

![[../../../img/Pasted image 20240125112001.png]]


# 事件方法

## Render的事件

![[../../../img/Pasted image 20240125112418.png]]

## 容器的事件

![[../../../img/Pasted image 20240125113123.png]]


## 字段组件的事件

![[../../../img/Pasted image 20240125113211.png]]