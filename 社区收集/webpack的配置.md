
基础配置

~~~js
const path = require('path');

module.exports = {
  mode: 'development',
  entry: './foo.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'foo.bundle.js',
  },
};
~~~


webpack只能理解JSON和js的语法。如果要其他的需要loader，转化为模块才可以使用。

# loader
在更高层面，在 webpack 的配置中，loader 有两个属性：

* test 属性，识别出哪些文件会被转换。
* use 属性，定义出在进行转换时，应该使用哪个 loader。

从右到左 从下到上

两种方式使用loader
配置方式（推荐）
>在 webpack.config.js 文件中指定 loader。

内联方式
>在每个 import 语句中显式指定 loader。


管方解读
~~~text
module.rules 允许你在 webpack 配置中指定多个 loader。 这种方式是展示 loader 的一种简明方式，并且有助于使代码变得简洁和易于维护。同时让你对各个 loader 有个全局概览：

loader 从右到左（或从下到上）地取值(evaluate)/执行(execute)。在下面的示例中，从 sass-loader 开始执行，然后继续执行 css-loader，最后以 style-loader 为结束。查看 loader 功能 章节，了解有关 loader 顺序的更多信息。
~~~

~~~js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: 'style-loader' },
          {
            loader: 'css-loader',
            options: {
              modules: true,
            },
          },
          { loader: 'sass-loader' },
        ],
      },
    ],
  },
};
~~~


# libraryTarget

![[../img/Pasted image 20240316111553.png]]


# target
![[../img/Pasted image 20240316111912.png]]