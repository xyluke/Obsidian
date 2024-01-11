# extensionVform
扩展的方法集合

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

