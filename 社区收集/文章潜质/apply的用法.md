# 手写实现apply
~~~js
function myApply(fn, context, args) {
  if (typeof fn !== 'function') {
    throw new TypeError('fn must be a function');
  }

  // 转换参数为真正的数组对象
  args = Array.isArray(args) ? args : Array.from(args);

  // 处理上下文对象
  context = context || window;

  // 创建唯一属性名
  const uniqueProp = Symbol('uniqueProp');

  // 将函数作为属性添加到上下文对象
  context[uniqueProp] = fn;

  // 执行目标函数
  const result = context[uniqueProp](...args);

  // 删除临时属性
  delete context[uniqueProp];

  // 返回执行结果
  return result;
}



// 使用示例
function greet(name) {
  console.log(`Hello, ${name}!`);
}

const person = {
  name: 'Alice'
};

myApply(greet, person, ['Bob']);  // 输出：Hello, Bob!
~~~

> 此实现与之前给出的示例相同，只是将其封装在 myApply 函数中以供调用。它会检查传入的函数是否可调用，并将传入的参数转换为真正的数组对象。然后，它会处理上下文对象，并在其上创建一个临时属性，将函数绑定到上下文对象上，并执行该函数。最后，它会删除临时属性并返回执行结果。

# 简易版的实现

~~~js
Function.prototype.apply = function (context, args) {
  if (typeof this !== 'function') {
    console.log('not a function')
  }
  const fn = Symbol()
  context[fn] = this
  context[fn](...args)
  delete context[fn]
}

~~~