# 要解决的问题
> 主要针对组件之间的跨级通信


# 为何要自己实现dispatch和broadcast
> 因为在做独立的组件开发或者库的时候，最好不要依赖第三方的库

# 为什么不使用provide和inject
> 因为他的使用场景，主要是子组件获取上级组件的状态，跨级组件间建立了一种主动提过与依赖注入的关系，然后有两种场景它不能很好的解决:
> 父组件向子组件（支持跨级）传递数据
> 子组件向父组件（支持跨级）传递数据

代码如下
~~~js
function broadcast(componentName, eventName, params) {
    this.$children.forEach(child => {
        let name = child.$options.componentName;

        if (name === componentName) {
            child.$emit.apply(child, [eventName].concat(params));
            broadcast.apply(child, [componentName, eventName].concat([params]));  //继续遍历子节点！！
        } else {
            broadcast.apply(child, [componentName, eventName].concat([params]));
        }
    });
}

export default {
    methods: {
        dispatch(componentName, eventName, params) {
            let parent = this.$parent || this.$root;
            let name = parent.$options.componentName;

            while (parent && (!name || name !== componentName)) {
                parent = parent.$parent;

                if (parent) {
                    name = parent.$options.componentName;
                }
            }
            if (parent) {
                parent.$emit.apply(parent, [eventName].concat(params));
            }
        },

        broadcast(componentName, eventName, params) {
            broadcast.call(this, componentName, eventName, params);
        }
    }
};
~~~