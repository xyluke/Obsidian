# 前言
接触Variant Form已经快一年了，一个代码量相对简洁的低代码设计工具。本着不记录就会忘记的原则，同时为以后的回顾轻松一点，写文章记录一下。

# 设计器结构
![[../../../img/Pasted image 20240124165415.png]]

这个设计器的核心文件就是`form-designer`

~~~js
form-designer （VFormDesigner设计器）
│
├─form-widget (设计器中部画布部分)
│  ├─container-widget (容器盒子组件)
│  └─field-widget    (字段功能组件)
│
├─setting-panel  (设计器右侧的编辑器部分)
│  ├─property-editor （所有右侧编辑器子集组件）
│  └─propertyRegister （子集组件注册和方法）
│
├─toolbar-panel (设计器中部头部的工具栏部分)
│
└─widget-panel (设计器左侧工具栏部分)
│
└─designer.js(设计器核心js文件)
│
└─index.vue(VFormDesigner vue文件)
│
└─refMixinDesign.js (混入的工具方法文件)
~~~

# 主要注意区块
## *设计器部分*
## designer.js
> 核心初始化文件，内部有很多管理整个设计器的逻辑方法。


## form-widget
> 内部有`container-widget`(容器设计组件)和`field-widget`（字段组件）
> 设计器将组件分为两个大板块
> 1. 容器 （例如：card、栅格、dialog弹窗等）
> 2. 字段（例如：button按钮、input输入框、单选、多选、日期等）


## setting-panel
> 编辑当前组件或者表单的编辑器部分。让使用者来配置自己的页面组件值。
> `property-editor-factory.js`
> 常用的构造器构建工具方法，如输入框、布尔单选器、复选框、下拉选择等
> 
> `propertyRegister`
> 右侧的编辑字段的注册和相对应的增添方法。分为三大类基础属性、高级属性、事件方法
> 给扩展新的字段留了添加口子的方法
> 
> 注意：
> `form`的设计编辑器直接是`form-setting.vue`文件


## widgetsConfig.js
> wiget-panel下面的文件，主要内容包含当前默认的各个组件的数据结构设计。
> 分为基础、高级、自定义这三大板块，与setting-panel相互结合构成动态的数据结构。


## toolbar-panel
> 导入导出，各个端的切换，清空返回等操作的区间。预览功能内部使用了vformRender组件。

## *渲染器部分*

## container-item
> 最终render渲染的容器组件的组件代码集合。在设计器阶段和渲染阶段他们是不一样的，但是字段基础组件大都是一样的。
> 
> 区别在设计器`card-widget`，渲染器则是`card-item`组件


# 扩展区块
> 如果有要后面添加新的组件则在`extesion`目录下的`widget`目录进行更改。
> 通过`extension-loader`内的`loadExtension`方法进行注册。

![[../../../img/Pasted image 20240124173548.png]]


# 多语言区块
> `lang`文件中记录着`en-US`和`zh-CN` 两种语言类型。进行语言切换


![[../../../img/Pasted image 20240124174327.png]]