
> Taro.navigateTo({url: '/pages/B/B?id='+id})


>B页面接收参数：
>1. 引入Taro
>2. 并在componentWillMount生命周期中通过Taro.getCurrentInstance()获取到路由对象，从中拿到参数
```
import Taro from '@tarojs/taro'        // 引入Taro
componentWillMount () { 
let id = Taro.getCurrentInstance()
.router.params.idconsole.log(id)
}
```
