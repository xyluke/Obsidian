# JS的Symbol大揭秘

~~~text
Symbol 的作用主要有以下几个方面：

创建唯一的属性名：Symbol 可以用作对象属性的键，它的值是独一无二的。这样可以确保属性名不会与其他属性名冲突，避免意外覆盖或修改属性。

防止属性名冲突：在使用第三方库或多人协作时，使用 Symbol 作为属性名可以避免不同模块之间的属性名冲突。每个模块可以使用自己的 Symbol 属性，而不会相互干扰。

定义类的私有成员：通过将 Symbol 作为类的私有成员，可以实现类似于私有属性和方法的概念。因为 Symbol 是独一无二的，外部无法直接访问到 Symbol 成员，从而保护了类的内部实现细节。

使用内置 Symbol 值扩展对象行为：JavaScript 提供了一些内置的 Symbol 值，用于定义对象的特殊行为。例如，使用 Symbol.iterator 可以使对象变得可迭代，使用 Symbol.toStringTag 可以自定义对象的 toString 方法返回的字符串。

实现自定义迭代器和生成器：通过使用 Symbol.iterator 创建迭代器，可以使对象成为可迭代的，并且可以使用 for...of 循环来遍历对象。此外，使用 Symbol.iterator 结合生成器函数可以自定义对象的迭代逻辑。

需要注意的是，Symbol 是一种不可变且唯一的数据类型，它和字符串、数字等其他类型的值不同。Symbol 值不能进行运算或转换为其他类型，它只能作为属性键来使用。
~~~

# 没有两个Symbol的值相等

~~~js
let a = Symbol('a');
let b = Symbol('b');
console.log(a == b) // false
console.log(a === b) // false
~~~

# 对象的键值可以是Symbol

~~~js
let name = Symbol('name1');

let a = {
	[name]: '张文',
	name: '李四',
	age: 24
}

console.log(a[name], a.name) // 张文 李四
~~~

# JSON.stringify() 会忽略Symbol类型
![[../../img/Pasted image 20231214163916.png]]


# symbol生成的值作为属性或者方法的时候，一定要保存下来，否则后续无法使用
~~~js
let name = Symbol('name');
let obj={ 
	name:'lnj',
	[Symbol('name')]:'lbj'
}
console.log(obj[name]); // undefined  
//访问不到，因为  [Symbol('name')]又是一个新的值
//和上面的name不是同一个


let name = Symbol('name');
let obj={ 
	name:'lnj',
	[name]:'lbj'
}
console.log(obj[name]); // lbj
~~~

# for循环遍历对象的时候是无法遍历出symbol的属性和方法
> 使用Object.getOwnPropertySymbols() 才能获取 获取的是包含symbol类型的key的数组
> Reflect.ownKeys() ：返回对象中所有类型key的数组（包含symbol）


~~~js
let name = Symbol('name');
let obj = {  
	[name]:'lnj',
	age:12,    
	teacher:'wyx'
}
for(let key in obj){
	console.log(key) 
}  
//只能打印出age和teacher
//这个方法可以单独取出Symbol(name)console.log(Object.getOwnPropertySymbols(obj))
~~~


# Symbol的一些方法

## Symbol.iterator
~~~js
class Song { 
	constructor(name, artist, duration){    
		this.name = name;    
		this.artist = artist;    
		this.duration = duration;  
	}
}
class Playlist {
  constructor() {
    this.songs = [];
  }

  addSong(song) {
    this.songs.push(song);
  }

  [Symbol.iterator]() {
    let index = 0;
    const songs = this.songs.slice().sort((a, b) => a.name.localeCompare(b.name)); // 按名称升序排序
    return {
      next: () => ({
        value: songs[index++],
        done: index > songs.length
      })
    }
  }
}

const playlist = new Playlist();
playlist.addSong(new Song('Song 3', 'Artist 3', '5:10'));
playlist.addSong(new Song('Song 1', 'Artist 1', '3:45'));
playlist.addSong(new Song('Song 2', 'Artist 2', '4:20'));

for (const song of playlist) {
  console.log(song.name);
}

~~~

~~~text
上述代码定义了两个类：Song 和 Playlist。Song 类表示歌曲，具有名称、艺术家和时长等属性。Playlist 类表示播放列表，通过数组来存储歌曲。

在 Playlist 类中，使用 addSong 方法向播放列表中添加歌曲。而 [Symbol.iterator] 方法是一个特殊的方法，用于定义对象的迭代器行为。在这里，它返回一个迭代器对象，该迭代器对象具有 next 方法，用于按顺序返回播放列表中的歌曲。

在主程序中，创建了一个名为 playlist 的播放列表实例，并分别向其中添加了三首歌曲。然后，使用 for...of 循环遍历 playlist 实例，每次循环都会从迭代器对象中获取下一首歌曲，并将其名称打印到控制台。

因此，执行以上代码会依次输出播放列表中每首歌曲的名称：
"Song 1"
"Song 2"
"Song 3"
这样就实现了自定义的迭代器逻辑，可以方便地遍历播放列表中的歌曲。
~~~

## Symbol.toStringTag

~~~js
class People {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  
  get [Symbol.toStringTag]() {
    return 'People';
  }
}

const people = new People('李小龙', 17);
console.log(people.toString()); // [object People]
~~~

~~~text
在这段代码中，我们定义了一个 People 类，它有两个属性 name 和 age，以及一个 getter 方法 Symbol.toStringTag。在 Symbol.toStringTag 的实现中，我们返回了字符串 'People'。

然后，我们创建了一个 People 类的实例 people，并使用 console.log() 方法输出了它的字符串表示。因为 People 类实现了 Symbol.toStringTag 方法，所以 console.log() 方法会自动调用该方法，并输出字符串 [object People]。这是因为，当我们在对象上调用 toString() 方法时，它会返回一个形如 [object Object] 的字符串表示，其中 Object 表示该对象的构造函数名称。但是，如果我们定义了 Symbol.toStringTag 方法，则可以自定义该字符串表示。

通过使用 Symbol.toStringTag，我们可以向 JavaScript 引擎提供更具描述性的对象类型信息。对于开发者来说，这样的信息更有意义，也更容易理解。
~~~

## Symbol.toPrimitive
> 可以被用来自定义对象类型转换时的行为。它可以接受一个 hint 参数，用于指示对象应该被转换成什么类型的值，比如 Number、String 或者其他默认的值。
> 
> 举一个实际的应用场景：假设我们正在开发一个日期处理工具，其中需要实现一个自定义的日期时间对象。这个对象包含日期时间的年月日时分秒等信息，这时候就可以用到 Symbol.toPrimitive 方法，来帮助我们自定义对象的类型转换行为：

~~~js
class MyDateTime {
  constructor(year, month, day, hour = 0, minute = 0, second = 0) {
    this._datetime = new Date(year, month - 1, day, hour, minute, second);
  }
  
  [Symbol.toPrimitive](hint) {
    switch (hint) {
      case 'number':
        return this._datetime.getTime();
      case 'string':
        return this._datetime.toLocaleString();
      default:
        return this._datetime.toString();
    }
  }
}

const myDate = new MyDateTime(2023, 4, 8, 15, 30, 0);
console.log(myDate); // 输出：Sat Apr 08 2023 15:30:00 GMT+0800 (中国标准时间)
console.log(myDate + 10000); // 输出：1699950200000
console.log(`${myDate}`); // 输出："2023/4/8 下午3:30:00"
~~~

~~~text
在这段代码中，我们定义了一个 MyDateTime 类，它有一个构造函数来接收年、月、日、时、分、秒参数。构造函数内部使用这些参数创建一个 Date 对象，并将其保存在 _datetime 属性中。

我们还实现了 Symbol.toPrimitive 方法，该方法在对象转换为原始值时会被调用。根据传入的 hint 参数，我们返回不同类型的原始值：

如果 hint 是 'number'，我们通过 getTime() 方法获取 _datetime 对应的时间戳。
如果 hint 是 'string'，我们通过 toLocaleString() 方法获取 _datetime 的本地化字符串表示。
对于其他 hint，我们调用 toString() 方法获取 _datetime 的默认字符串表示。
然后，我们创建了一个 MyDateTime 类的实例 myDate。通过 console.log() 打印 myDate，它会自动调用对象的 toString() 方法，并输出默认的字符串表示。

接下来，我们使用 + 运算符将 myDate 和 10000 相加。因为 myDate 实现了 Symbol.toPrimitive 方法，它会被转换为原始值，而不是进行默认的对象相加操作。

最后，我们使用模板字符串将 myDate 转换为字符串，并输出其内容。

这样，我们可以根据需要自定义对象在不同上下文中的默认行为，使其更加灵活和适应多种场景。
~~~


## Symbol.asyncIterator

> Symbol.asyncIterator 可以用来实现一个对象的异步迭代器，它可以用于遍历异步数据流，比如异步生成器函数、异步可迭代对象等。这个特性在我们需要处理异步数据流时非常有用。
> 
> 举一个实际的应用场景：假设我们正在开发一个异步数据源处理器，其中包含了大量的异步数据，比如网络请求、数据库查询等。这些数据需要被逐个获取并处理，同时由于数据量非常大，一次性获取全部数据会导致内存占用过大，因此需要使用异步迭代器来逐个获取数据并进行处理：

~~~js
class AsyncDataSource {
  constructor(data) {
    this._data = data;
  }
  
  async *[Symbol.asyncIterator]() {
    for (const item of this._data) {
      const result = await this._processAsyncData(item);
      yield result;
    }
  }
  
  async _processAsyncData(item) {
    // 模拟异步处理数据的过程
    return new Promise((resolve) => {
      setTimeout(() => {
        resolve(item.toUpperCase());
      }, Math.random() * 1000);
    });
  }
}

async function processData() {
  const dataSource = new AsyncDataSource(['a', 'b', 'c', 'd', 'e']);
  for await (const data of dataSource) {
    console.log(data);
  }
}

processData();

~~~

~~~text
这段代码定义了一个 AsyncDataSource 类，它接收一个数组作为数据源，实现了 Symbol.asyncIterator 方法以支持异步迭代。

在 *[Symbol.asyncIterator]() 方法中，我们使用 for...of 循环遍历 _data 数组，并调用 _processAsyncData() 方法对每个元素进行异步处理。由于 _processAsyncData() 方法返回的是 Promise 对象，我们需要使用 await 等待其执行完成后才能继续执行下一轮循环，并通过 yield 返回处理结果。

_processAsyncData() 方法内部使用 setTimeout() 模拟异步处理数据的过程。它会随机生成一个延时时间，在延时结束后将元素转换为大写字母并通过 Promise 的 resolved 状态返回。

最后，我们定义了一个 processData() 异步函数，它创建了一个 AsyncDataSource 实例并使用 for await...of 循环遍历异步迭代器返回的数据，并将每个数据项打印到控制台上。

通过在类中实现 Symbol.asyncIterator 方法和异步处理逻辑，我们可以轻松地将同步数据源转换成支持异步迭代的数据源，从而实现更加灵活和高效的数据处理。
~~~


## Symbol.hasInstance
> Symbol.hasInstance可以用于确定一个对象是否是某个构造函数的实例，它可以用来改变 instanceof 的行为：

~~~js
class MyArray {
  static [Symbol.hasInstance](instance) {
    return Array.isArray(instance);
  }
}

const arr = [1, 2, 3];
console.log(arr instanceof MyArray); // true
~~~

~~~text
这段代码定义了一个 MyArray 类，并实现了一个静态方法 [Symbol.hasInstance]。在这个方法中，我们通过 Array.isArray() 检查传入的实例是否为数组，如果是则返回 true，否则返回 false。

由于 Symbol.hasInstance 是一个内置的 Symbol，它可以让我们自定义一个对象的 instanceof 行为，使其能够与我们定义的类进行比较。当使用 instanceof 运算符检查一个实例时，JavaScript 引擎会调用该实例的 constructor[Symbol.hasInstance](instance) 方法来判断实例是否属于该类。

在上面的代码中，我们创建了一个普通数组 arr，然后使用 instanceof 运算符检查它是否是 MyArray 的实例。由于 MyArray 类的 [Symbol.hasInstance] 方法被调用并返回了 true，所以 arr instanceof MyArray 的结果为 true。

通过自定义 [Symbol.hasInstance] 方法，我们可以为类添加更加灵活和多样化的类型判断逻辑，从而扩展 JavaScript 中原有的 instanceof 运算符的功能。
~~~

## Symbol.species
> Symbol.species 可以用于指定创建派生对象时要使用的构造函数。一个实际的需求场景可能是我们需要对一个类进行继承，并且希望这个类的某些方法返回一个新的派生对象而不是基类的实例对象。下面是一个简单的例子：

~~~js
class MyArray extends Array {
  static get [Symbol.species]() {
    return Array;
  }
  
  test() {
    console.log('test');
  }
}

const myArray = new MyArray(1, 2, 3);
const mappedArray = myArray.map(x => x * 2);

myArray.test(); // 输出 'test'
console.log(mappedArray instanceof MyArray); // false
console.log(mappedArray instanceof Array); // true

~~~

~~~text
这段代码定义了一个 MyArray 类，它继承自 JavaScript 的内置 Array 类。通过继承 Array 类，MyArray 类获得了所有 Array 类的方法和特性。

在 MyArray 类中，我们通过静态属性 [Symbol.species] 返回了 Array，这个属性定义了一个类的派生类应该使用的构造函数。在这里，我们指定派生类 MyArray 使用 Array 构造函数创建新的实例。

接下来，我们创建了一个 myArray 实例，并传入了初始值 1、2、3。然后，我们调用 map() 方法对 myArray 进行映射操作，将每个元素都乘以 2，并将结果赋给 mappedArray 变量。

此外，我们在 MyArray 类中定义了一个名为 test() 的方法，用于输出字符串 'test'。

然后，我们分别调用了 myArray.test() 和输出了两个 instanceof 的结果。由于 myArray 是 MyArray 类的实例，所以调用 myArray.test() 会输出 'test'。而 mappedArray 是通过 map() 方法生成的新数组，它的原型链上的构造函数不再是 MyArray，因此 mappedArray instanceof MyArray 的结果为 false，而 mappedArray instanceof Array 的结果为 true。

通过使用继承和 [Symbol.species] 属性，我们可以扩展和定制 JavaScript 数组，并灵活地控制派生类的行为。
~~~

##  Symbol.match
> Symbol.match 可以用来确定使用 String.prototype.match 方法时要搜索的值，它可用于更改 match 类 RegExp 对象的方法行为，下面是一个实际实用案例：

~~~js
class CustomRegExp extends RegExp {
  [Symbol.match](str) {
    const matches = super[Symbol.match](str);
    if (matches) {
      return matches.map(match => {
        return `匹配到了: ${match}`;
      });
    }
    return matches;
  }
}

const regex = new CustomRegExp('hello', 'g');
const result = 'hello world'.match(regex);
console.log(result); // ["匹配到了: hello"]

~~~

~~~text
这段代码定义了一个 CustomRegExp 类，它继承自 JavaScript 的内置 RegExp 类。通过继承 RegExp 类，CustomRegExp 类可以使用正则表达式的功能。

在 CustomRegExp 类中，我们重写了 [Symbol.match] 方法。该方法用于在字符串中执行正则表达式的匹配，并返回匹配结果。

在重写的 [Symbol.match] 方法中，我们首先调用 super[Symbol.match](str)，即调用父类 RegExp 的 [Symbol.match] 方法，获取原始的匹配结果。

如果原始的匹配结果存在（即非空），我们将对每个匹配项进行处理，使用 map() 方法将每个匹配项转换成特定格式的字符串，添加前缀 "匹配到了: "。

最后，返回处理后的匹配结果数组。

在代码的后续部分，我们创建了一个 CustomRegExp 实例 regex，并传入模式字符串 'hello' 和标志 'g'。然后，我们使用字符串 'hello world' 调用 match() 方法，并传入 regex 实例作为参数，执行匹配操作。

最后，我们将匹配结果输出到控制台，结果为 ["匹配到了: hello"]。这是因为我们重写的 [Symbol.match] 方法对匹配结果进行了处理，添加了前缀 "匹配到了: "。
~~~

## Symbol.replace
> Symbol.replace 可以帮我们更灵活的处理 String.prototype.replace 方法，比如我们可以自定义字符串的替换行为。
> 
> 比如有这样一个需求场景：我们有一个字符串处理库，我们想自定义它的 replace 方法，让它可以替换一个字符串的所有元音字母。这时候就可以用到 Symbol.replace :

~~~js
const vowels = ['a', 'e', 'i', 'o', 'u'];

const customReplace = str => {
  let result = '';
  for (let i = 0; i < str.length; i++) {
    if (vowels.includes(str[i])) {
      result += '*';
    } else {
      result += str[i];
    }
  }
  return result;
};

const customString = {
  [Symbol.replace]: customReplace
};

const originalString = "hello world";
const result = originalString.replace(customString, '');
console.log(result); // 输出 "h*ll* w*rld"

~~~

~~~text
这段代码首先定义了一个包含元音字母的数组 vowels，然后定义了一个名为 customReplace 的函数，用于将字符串中的元音字母替换为 *。

在 customReplace 函数中，我们遍历输入的字符串，对于每个字符，如果它是元音字母，则用 * 替换，否则保持原样，最后返回替换后的结果。

接下来，我们创建了一个名为 customString 的对象，并使用 ES6 的计算属性名语法，将 Symbol.replace 属性指定为前面定义的 customReplace 函数。

然后，我们定义了一个原始字符串 originalString，内容为 "hello world"。接着，我们调用原始字符串的 replace() 方法，将 customString 作为参数传入，执行替换操作。

最后，我们将替换后的结果输出到控制台，结果为 "h*ll* w*rld"。这是因为我们使用 customString 对象作为替换规则，将字符串中的元音字母替换为 *，得到了相应的输出结果。
~~~

## Symbol.split
> Symbol.split 可以用来确定使用 String.prototype.split 方法执行时具体要拆分的值。
> 
> 一个实际需求场景：我们需要从文本中提取出所有的数字。但是文本中的数字可能包含在不同的字符和符号中，例如括号、分隔符和单位等。使用 Symbol.split 可以自定义分割符，这样我们就可以根据自己的需求来对文本进行分割。

~~~js
const customSplit = str => str.split(/[\s$¥€]+/);

const customRegExp = {
  [Symbol.split]: customSplit
};

const string = "100$200¥300€400 500";
console.log(string.split(customRegExp)); // 输出 [ '100', '200', '300', '400', '500' ]
~~~

~~~text
这段代码定义了一个名为 customSplit 的函数，用于将字符串按照空格、美元符号、日元符号和欧元符号进行分割。在这个函数中，使用了正则表达式 /[\s$¥€]+/ 来匹配空格、美元符号、日元符号和欧元符号的连续出现。

接下来，我们创建了一个名为 customRegExp 的对象，并使用计算属性名语法，将 Symbol.split 属性指定为前面定义的 customSplit 函数。

然后，我们定义了一个字符串 string，内容为 "100$200¥300€400 500"。接着，我们调用字符串的 split() 方法，并将 customRegExp 对象作为参数传入，执行分割操作。

最后，我们将分割后的结果输出到控制台，结果为 [ '100', '200', '300', '400', '500' ]。这是因为我们使用 customRegExp 对象作为分割规则，按照空格、美元符号、日元符号和欧元符号进行分割，得到了相应的输出结果。
~~~

## Symbol.unscopables
> Symbol.unscopables 通常可以用来避免在使用 with 语句时访问对象中不希望被访问的属性。下面是一个示例，其中使用 with 语句访问对象的某些属性，但通过将这些属性添加到 [Symbol.unscopables] 对象中，可以防止访问：

~~~js
const globalVars = {
  a: 10,
  b: 20,
  [Symbol.unscopables]: {
    b: true
  }
};

with (globalVars) {
  console.log(a); // 输出 10
  console.log(b); // 抛出 ReferenceError
}
~~~

~~~text
这段代码首先定义了一个名为 globalVars 的对象，包含两个属性 a 和 b，分别赋值为 10 和 20。此外，我们使用了计算属性名的方式，定义了一个名为 Symbol.unscopables 的属性，该属性的值是一个对象，其中的属性 b 被设置为 true。

接下来，我们使用 with 语句引入了 globalVars 对象作为作用域，在这个作用域中，我们通过 console.log 分别输出了 a 和 b 的值。

由于 Symbol.unscopables 属性指定了 b 在 with 语句中不可访问，因此当我们尝试输出 b 的值时，会抛出 ReferenceError。而对于属性 a，可以正常输出其值，结果为 10。
~~~


# 大佬语录
> 一台电脑,一个键盘,尽情挥洒智慧的人生;几行数字,几个字母,认真编写生活的美好;
> 一 个灵感,一段程序,推动科技进步,促进社会发展。

[文章链接](https://baijiahao.baidu.com/s?id=1762860617098457132&wfr=spider&for=pc)
