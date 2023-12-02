>Vuex可以将store分割成模块。每个模块拥有自己的state、mutations、actions、getters、modules

> 定义store的modules
# 利用registerModule方法

~~~js
// 注册模块 `myModule`
store.registerModule('myModule', {
  // ...
})
// 注册嵌套模块 `nested/myModule`
store.registerModule(['nested', 'myModule'], {
  // ...
})
~~~

> 之后就可以通过 store.state.myModule 和 store.state.nested.myModule 访问模块的状态。
> 
> 模块动态注册功能使得其他 Vue 插件可以通过在 store 中附加新模块的方式来使用 Vuex 管理状态。例如，vuex-router-sync 插件就是通过动态注册模块将 vue-router 和 vuex 结合在一起，实现应用的路由状态管理。
> 
> 你也可以使用 store.unregisterModule(moduleName) 来动态卸载模块。注意，你不能使用此方法卸载静态模块（即创建 store 时声明的模块)
