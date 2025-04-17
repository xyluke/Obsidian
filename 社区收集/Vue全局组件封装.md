## 封装原因

为什么要全局封装？

~~~text
项目中需要多次使用，进行一次封装，全局使用。迎合了es6的模块化开发思想。
~~~

## 封装步骤

### 方法一

1.在components文件夹下面 添加一个`global.js`的文件

2.在该文件内部编写以下代码

准备一个已经封装好的组件

~~~vue
<template>
  <el-button type="primary" icon="el-icon-plus" :disabled="disabled">
  {{label}}
  </el-button>
</template>
<script>
export default {
  name: "button-add",
  props: {
    label: {
      type: String,
      default: "添加按钮",
    },
    disabled: {
      type: Boolean,
      default: false,
    },
  },
};
</script>
~~~

在`global.js`文件下面进行导入编写 

~~~js
import Vue from "vue";
//添加按钮的封装
import ButtonAdd from "@/components/button/ButtonAdd.vue";

Vue.component('AddButton',ButtonAdd)
~~~

3.在`main.js`的下导入全局组件

~~~js
import Vue from 'vue'
import App from './App.vue'
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';

Vue.use(ElementUI)
Vue.config.productionTip = false

// 引入自己封装的全局组件
import '@/components/register-global-components.js'

new Vue({
  render: h => h(App),
}).$mount('#app')
~~~

4.在`vue`的文件里面直接使用就可以了

~~~vue
<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <el-button type="primary">测试按钮</el-button>
    <add-button label="修改了按钮" ></add-button>
  </div>
</template>
~~~

### 方法二

思路:通过注册插件的方式来注册全局组件

~~~text
Vue官方提供的插件有 Vue Router、Vuex和Vue 服务端渲染三个
Vue.use可以接受一个对象，Vue.use(obj)
对象obj中需要一个 install 函数
在 Vue.use(obj)时，会自动调用该 install 函数，并传入到 Vue构造器
~~~



1.在`src/components/dialog`准备一个组件`DialogBasic`

~~~vue
<template>
  <el-dialog :title="title" :visible.sync="dialogVisible" :width="width">
    <slot></slot> 
    <span slot="footer" class="dialog-footer">
      <el-button @click="dialogVisible = false">取 消</el-button>
      <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
    </span>
  </el-dialog>
</template>
<script>
export default {
  name: "my-dialog",
  props: {
    title: {
      type: String,
      default: "默认提示信息",
    },
    width: {
      type: String,
      default: "50%",
    },
    visible: {
      type: Boolean,
      default: true,
    },
  },
  data() {
    return {
      dialogVisible: this.visible,
      user: {
        name: "弹窗的名字",
        lastname: "弹框LastName",
      },
    };
  },
};
</script>
~~~

2.在`src/components`文件夹下面新建一个`index.js`文件

~~~js
// 导入自己封装好的组件
import MyDialog from '@/components/dialog/DialogBasic.vue'

export default {
    install:function(Vue){
        console.log('自定义的插件执行');
        Vue.component('MyDialog',MyDialog)
    }
}
~~~

3.在`main.js`文件下面导入自己定义的全局组件`index.js`

~~~js
import Vue from 'vue'
import App from './App.vue'
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';

Vue.use(ElementUI)
Vue.config.productionTip = false

// 引入自己封装的全局组件
import '@/components/register-global-components.js'

//第二种方法引入自己的全局组件
import myCom from '@/components'
//用使用插件的方式进行注册
Vue.use(myCom)

new Vue({
  render: h => h(App),
}).$mount('#app')

~~~

4.在`vue`组件里面进行使用即可

~~~vue
<my-dialog title="提醒头部改变了">
      <template #default>
			<!--插槽里面的内容-->
      </template>
</my-dialog>
~~~



## 插槽的补充

> 规则:父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。

### 具名插槽

~~~text
就是插槽定特定的名字
<template>
    <slot name="head"></slot>
    <slot name="foot"></slot>
</template>

不带内容的slot 则是带了隐藏属性 <slot name="default"></slot>
~~~

使用时

~~~vue
<template>
	<template v-slot:head>
	<!--则会替换掉插槽head里面的内容,并且默认的插槽会被具名插槽替代掉。-->
	</template>
	<template #foot>
	<!--#是v-slot的语法糖，#foot 等价于 v-slot:foot-->
	</template>
</template>
~~~

### 作用域插槽

场景：为了让组件里面的user在父组件里面也能使用，将user进行一个attribute绑定上去

~~~vue
<template>
	<slot v-bind:user="user">
    {{user.name}}
    </slot>
</template>

<script>
export default{
    name:'Test',
    data(){
        return{
            user:{
                name:'插槽自己原本的内容--本身',
                lastname:'插槽想要更换的内容--本身'
            }
        }
    }
}
</script>
~~~

* 绑定在<slot>元素上的attribute被称为插槽prop。现在在父级作用域中使用带值的v-slot 来定义我们提供的插槽prop的名字
~~~json
<template>
    <test>
        <template v-slot:default="slotProps">
			{{slotprops.user.firstname}}
		</template>
    </test>
</template>
~~~

 

#### 解构插槽Prop

作用域插槽的内部工作原理是将你插槽内容包裹在一个拥有单个参数的函数里

~~~js
function(slotProps){
    //插槽内部的内容
}
~~~

因此 `v-slot`的值就是可以是 原先函数能够接受的参数类型值都可以传递 ，因此对象可以采用解构

~~~html
<test>
    <template v-slot="{user}">
    	{{user}}
    </template>
</test>
~~~

### 动态插槽名

[动态指令参数](https://cn.vuejs.org/v2/guide/syntax.html#动态参数)也可以用在 `v-slot` 上，来定义动态的插槽名：

~~~html
<base-layout>
  <template v-slot:[dynamicSlotName]>
    ...
  </template>
</base-layout>
~~~



## Vue自定模板

步骤:用户自定义片段-->输入vue-->放入下面代码即可

~~~json
{        
	"vue-template": {            
		"prefix": "vueTemp",            
		"body": [                
			"<template>",                
			"  <div>",                
			"",                
			" </div>",                
			"</template>",                
			"",                
			"<script>",                
			"export default {",                
			"  name: '$TM_FILENAME_BASE',",                
			"  data() { ",                
			"    return {",                
			"",                
			"    }",                
			"  },",                             
			"  components:{",               
 			"  },",                   
			"  methods:{",       
			"",        
            "  },",
            "  mounted() {",               
			"",             
			"  },",            
			" }",        
			"</script>",      
			 "",       
			 "<style>",                               
			"</style>"               
		],         
		"description": "my vue template"  
	}  
}
~~~

