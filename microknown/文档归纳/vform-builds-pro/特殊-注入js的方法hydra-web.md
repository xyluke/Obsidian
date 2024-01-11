# extensionVform
扩展的方法集合
![[../../../img/Pasted image 20240111193209.png]]


index.js
~~~js
const requireContext = require.context('./', true, /^\.\/(?!index\.js).*\.js$/);
const modules = requireAll(requireContext);
// console.log(modules.expense.getAllFunction());

function requireAll(requireContext) {
    const modules = {};

    requireContext.keys().forEach((key) => {
        // 忽略当前的 index.js 文件
        if (key === './index.js') {
            return;
        }

        let moduleKey = key.replace(/\.\/(.*)\.\w+$/, '$1').replace(/\.vform/, '').replace(/\//, '_');
        modules[moduleKey] = requireContext(key).default;
    });

    return modules;
}

export default modules;
~~~



# 在template-vform-detail-version1中使用

~~~js
import extensionAllMethod from "@/ux/extensionVform";
~~~

![[../../../img/Pasted image 20240111193448.png]]

![[../../../img/Pasted image 20240111193604.png]]

~~~js
filterWidgetList(widgetList, methodCode) {
	// 获取所有注入方法 extensionVform
	let extensionVform = extensionAllMethod;
	let allFunArray = []
	// getAllFunction是类中的static 类型的方法
	if (extensionVform[methodCode] && extensionVform[methodCode].getAllFunction) {
		allFunArray = extensionVform[methodCode] && extensionVform[methodCode].getAllFunction && extensionVform[methodCode].getAllFunction();
		// globalExternal这个全局扩展方法：当vform内部想要使用hydra-web的通用方法时的口子
		Object.assign(this, extensionVform[methodCode].globalExternal())
	}

	let widget = widgetList;
	widget && widget.forEach(item => {
		// 1. 容器
		if (item.category === 'container') {
			if(item.type === 'grid') {
				this.filterWidgetList(item.cols, methodCode);
			} else {
				this.filterWidgetList(item.widgetList, methodCode);
			}
		} else {
			// 字段
			if (!!item.options.name) {
				let injectData = allFunArray?.find(item2 => item2.code === item.options.name);
				if (injectData) {
					Object.assign(item.options, injectData);
				}
			}
		}
	})
},
~~~