# 题目
> 已知 ES5 中 func.bind(context, 1, 2)(3, 4) 等价于 func.call(context, 1, 2, 3, 4) 请用 ES3 实现一个 bind 的 polyfill

> 在前端的面试中，经常会问到call, apply, bind方法的区别？
	call: fn.call(context, 1, 2, 3, 4)
	apply: fn.apply(context, [1, 2, 3, 4])
	bind: fn.bind(context, 1, 2)(3, 4)

> context为this的指向, 以上三种方法执行的结果一致，call、apply两者的区别就是传参方式的不同，而bind方法执行结果返回的是一个未执行的方法，执行时可以继续传入参数，实现了函数的柯里化，提高参数的复用
# 手写bind函数

~~~js
Function.prototype.myBind = function (context = window) { 
// 原型上添加mybind方法
// var args = Array.from(arguments) 
// 类数组转数组(es6) console.log(args instanceof Array)
   var argumentsArr = Array.prototype.slice.call(arguments) // 类数组转数组
   var args = argumentsArr.slice(1) // 后面的参数 从1开始取值
   var self = this // 调用的方法本身
   return function () { // 返回一个待执行的方法
       var newArgs = Array.prototype.slice.call(arguments) // 返回函数的arguments,类数组转数组或者使用es6解构赋值
       self.apply(context, args.concat(newArgs)) // 合并两args
   }
}


~~~

## 修改保证原型链不断
~~~js
Function.prototype.MyBind = function(context){
    var self = this
    var args = Array.prototype.slice.call(arguments, 1)
    var temp = function () {}
    var fn = function () {
        var newArgs = Array.prototype.slice.call(arguments)
        // 如果被new调用，this应该是fn的实例
        return self.apply(this instanceof fn ? this : (context || window), args.concat(newArgs) )
    }
    // 维护fn的原型
    temp.prototype = self.prototype
    fn.prototype = new temp()
    return fn
}
~~~


修改版
~~~js
  Function.prototype.MyBind = function(context){
	if (typeof this !== "function") {
	    console.log("Error");
	}
    var self = this
    var args = Array.prototype.slice.call(arguments, 1)
    var temp = function () {}
    var fn = function () {
        var newArgs = Array.prototype.slice.call(arguments)
        // 如果被new调用，this应该是fn的实例
        return self.apply(this instanceof fn ? this : (context || window), args.concat(newArgs) )
    }
    // 维护fn的原型
    temp.prototype = self.prototype
    fn.prototype = new temp()
    return fn
}
~~~
# 测试结果

~~~js
//  测试
var name = 'window name'
var obj = {
    name: 'obj name',
}
var fn = function () {
    console.log(this.name, [...arguments])
}
fn(1, 2, 3, 4) // 直接执行，this指向window
fn.myBind(obj, 1, 2)(3, 4) // mybind改变this指向
fn.bind(obj, 1, 2)(3, 4) // 原生bind

// 以上执行结果如下：
// window name [1, 2, 3, 4]
// obj name [1, 2, 3, 4]
// obj name [1, 2, 3, 4]
~~~




# 疑问
## 1. 为什么bind返回的函数对象的this的指向不能再改变了？

~~~js
Function.prototype.MyBind = function(context){
    var self = this
    var args = Array.prototype.slice.call(arguments, 1)
    var temp = function () {}
    var fn = function () {
        var newArgs = Array.prototype.slice.call(arguments)
        // 如果被new调用，this应该是fn的实例
        return self.apply(this instanceof fn ? this : (context || window), args.concat(newArgs) )
    }
    // 维护fn的原型
    temp.prototype = self.prototype
    fn.prototype = new temp()
    return fn
}
function saySomething(something) {
  console.log(this.name + ' says: ' + something);
}

let person1 = { name: 'Alice' };
let person2 = { name: 'Bob' };

let boundFunc1 = saySomething.MyBind(person1);
let boundFunc2 = boundFunc1.MyBind(person2);

boundFunc1('Hello!'); // 输出 "Alice says: Hello!"
boundFunc2('Hi there!'); // 输出 "Alice says: Hi there!"
~~~

在 JavaScript 中，使用 bind 方法绑定函数后，再次调用 bind 方法会产生一个新的绑定函数，而不会改变原始绑定函数的行为。这是因为 bind 方法返回的是一个新的函数，而不是修改原始函数的行为。

在这个例子中，我们首先将 saySomething 函数绑定到 person1 上，得到了 boundFunc1。然后，我们再次将 boundFunc1 绑定到 person2 上，得到了 boundFunc2。尽管我们对 boundFunc1 进行了第二次绑定，但它仍然会保持原始绑定的行为，而不会改变其 this 指向。

因此，每次调用 bind 方法都会产生一个新的绑定函数，而不会改变原始函数或之前创建的绑定函数。

> bind 方法返回的绑定函数是一个新的函数对象，它的 this 指向被永久固定在绑定时指定的值。这种不可更改的特性有以下原因：

~~~text
一致性和预测性：JavaScript 中的函数调用机制是基于执行上下文（this 值）的，通过将函数绑定到特定的对象上，可以确保函数在后续调用中始终具有一致的行为。这种固定的 this 值可以提供可预测性，使得代码更加易于理解和调试。

避免意外行为：如果允许绑定函数的 this 值再次更改，可能会导致代码的不一致和难以调试的问题。固定绑定函数的 this 值可以确保代码的行为符合预期，并且减少了出现错误的可能性。

内部实现原理：bind 方法的内部实现通常使用了闭包和函数的内部属性，这些实现技巧使得绑定的 this 值成为不可变的。这种设计决策是为了确保函数的一致性和性能。

虽然你不能直接更改已绑定函数的 this 值，但你仍然可以通过其他方式来间接地改变它，比如使用 call、apply 或者箭头函数等。这些方法可以创建一个新的执行上下文，并在调用函数时指定新的 this 值。

总结起来，bind 方法返回的绑定函数的 this 指向被固定是为了保持代码的一致性、可预测性和避免意外行为，并且与内部实现原理有关。
~~~