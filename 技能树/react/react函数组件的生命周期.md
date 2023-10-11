react的函数组件没有类组件的生命周期函数。但是有了新的特性React Hooks。 有些钩子函数具备类似生命周期函数的功能。
1. useEffect： useEffect是常用的钩子函数之一。在组件挂载后执行，可以用来处理副作用操作（数据获取、订阅事件）

~~~js
import React, { useEffect } from 'react'; 
function MyComponent() { 
	useEffect(() => { 
		// 组件挂载时执行的逻辑 
		return () => { 
			// 组件卸载时执行的逻辑 
		}; 
	}, []); // 空依赖数组表示只在组件挂载和卸载时执行 
	
	return ( 
		// 组件的 JSX 内容 
	); 
}
~~~