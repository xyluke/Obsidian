# 场景
vue中存有mixin这个方法，在低代码中mixin的混入把字段组件和容器组件的公用方法进行了提取。现在要将这种思想放在react中，要如何实现呢？

# Mixin


# HOC
`React`高阶组件，参数是一个组件，返回值也是一个组件。
作用： 强化组件、复用逻辑、提升渲染性能作用。
用来解答问题：
> 如何创建HOC组件
> 如何使用
> 如何获取里面的静态方法，且在内部使用

## 原型图
![[../../../img/Pasted image 20240127104519.png]]


# 两种不同的HOC
## 正向代理

~~~js
const withMixin = (WrappedComponent) => {
    class MixinComponent extends Component<PropsType, StateType> {
        constructor(props) {
            super(props);
            this.state = {
                name: 'admin'
            }
        }

        componentDidMount() {
            console.log(this.state.name);
        }

        say(msg) {
            console.log(msg, this.state);
        }

        handleInputCustomEvent(value) {
            // 此处为formModel更改值
            this.syncUpdateFormModel(value);
            if(!!this.props.field.options.onInput) {
                let customFn = new Function('value', this.props.field.options.onInput);
                customFn.call(this, value);
            }
        }

        syncUpdateFormModel(value) {
            console.log(value, '异步更改value的值');
        }

        render() {
        return <WrappedComponent 
        {...this.props} 
        handleInputCustomEvent={this.handleInputCustomEvent}
        name={this.state.name}/>;
        }
    }

    return MixinComponent;
}

export withMixin


// 使用hoc的组件
import mixIn from "./mixIn";
class InputWidget extends Component<MyProps> {

	render(){
		return (
		<div>
			<span>使用hoc的组件</span>
			<span>{this.props.name}</span>
			<span>input点击的方法</span>
			<input onClick={this.props.handleInputCustomEvent} />
		</div>
		)
	}
}

const InputWidgetWithMixin = mixIn(InputWidget);
export default InputWidgetWithMixin;
~~~

## 反向代理
反向继承和属性代理有一定的区别，在于包装后的组件继承了业务组件本身，所以我们我无须在去实例化我们的业务组件。当前高阶组件就是继承后，加强型的业务组件。这种方式类似于组件的强化，所以你必要要知道当前。
~~~js
const withMixin = (WrappedComponent) => {
    return class MixinComponent extends WrappedComponent<PropsType, StateType> {
	    // 如果要在这里加state就最好注入进去，因为constructor会覆盖这个东西
        say(msg) {
            console.log(msg);
        }

        handleInputCustomEvent(value) {
            // 此处为formModel更改值的方法
        }
    }
}

export default withMixin;

// 使用hoc的组件
import mixIn from "./mixIn";
class InputWidget extends Component<MyProps> {

	render(){
		return (
		<div>
			<span>使用hoc的组件</span>
			<span>input点击的方法</span>
			<input onClick={this.handleInputCustomEvent} />
		</div>
		)
	}
}

const InputWidgetWithMixin = mixIn(InputWidget);
export default InputWidgetWithMixin;
~~~


