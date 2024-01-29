[单纯js获取redux的值](https://blog.csdn.net/qq_42339350/article/details/113664507)

~~~js
import store from "@/model/index";

store.dispatch('引入的声明store的方法');
store.getState().modelName  //'model的名字'

~~~