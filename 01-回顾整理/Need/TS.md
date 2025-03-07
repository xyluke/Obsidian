在线ts网站
https://www.typescriptlang.org/zh/play/?#code

![[../../img/Pasted image 20250226141517.png]]

![[../../img/Pasted image 20250226141528.png]]

![[../../img/Pasted image 20250226141541.png]]
![[../../img/Pasted image 20250226141755.png]]
![[../../img/Pasted image 20250226141805.png]]
![[../../img/Pasted image 20250226141819.png]]
![[../../img/Pasted image 20250226141828.png]]
![[../../img/Pasted image 20250226141837.png]]
![[../../img/Pasted image 20250226141845.png]]![[../../img/Pasted image 20250226141854.png]]
![[../../img/Pasted image 20250226141908.png]]
![[../../img/Pasted image 20250226141918.png]]![[../../img/Pasted image 20250226141929.png]]![[../../img/Pasted image 20250226141939.png]]![[../../img/Pasted image 20250226141950.png]]
![[../../img/Pasted image 20250226141958.png]]![[../../img/Pasted image 20250226142010.png]]
![[../../img/Pasted image 20250226142020.png]]

# 泛型的作用
主要目的是提高代码的 可复用性和类型安全性
![[../../img/Pasted image 20250226152400.png]]
![[../../img/Pasted image 20250226152417.png]]
![[../../img/Pasted image 20250226152435.png]]
![[../../img/Pasted image 20250226152447.png]]


* 避免冗余的代码和类型声明
>在传统的 JavaScript 或 TypeScript 中，如果没有泛型，你可能需要为每个类型编写单独的函数或者多个重载来处理不同类型的数据。泛型通过提供类型参数，可以避免大量的冗余代码。
~~~js
function showNumber(n: number): number {
  return n;
}

function showString(s: string): string {
  return s;
}

// 使用泛型后，只需一个函数：
function showValue<T>(value: T): T {
  return value;
}

let numberVal = showValue(5);  // number
let stringVal = showValue("hello");  // string
~~~
* 与类、接口结合使用
>泛型不仅可以用于函数，也能在类、接口等复杂数据结构中使用。比如说，你可以编写一个泛型类来处理不同类型的数据，这种方式允许你创建可重复使用的类，这些类可以操作多种不同类型的数据，而无需写多个类

~~~js
class Box<T> {
  value: T;
  constructor(value: T) {
    this.value = value;
  }
}

let numberBox = new Box(123);  // Box<number>
let stringBox = new Box("hello");  // Box<string>
~~~

# 修饰符
![[../../img/Pasted image 20250226143654.png]]

readonly只读修饰符修改
~~~js
方法一
let person:P = {
    name: '流线',
    age: 20,
    run: (p) => {
        console.log(p);
    }
};

(person as any).age = 178;
~~~
![[../../img/Pasted image 20250226145509.png]]

![[../../img/Pasted image 20250226145309.png]]

![[../../img/Pasted image 20250226145338.png]]

![[../../img/Pasted image 20250226145349.png]]
![[../../img/Pasted image 20250226145426.png]]
# 类型强转
![[../../img/Pasted image 20250226144252.png]]


# Interface
~~~ts
interface animal {
  name: string,
  eat(name: string): string,
  run?(): void
}

interface dog extends animal {
  dogName: string
}
const smartDog : dog = {
  dogName: '88',
  name: '狗',
  eat(name:string){
    console.log(name);
    return name
  }
}
smartDog.eat('骨头')
~~~
作用：
1. 对象类型定义
~~~js
interface Person {
  name: string;
  age: number;
}

const person: Person = {
  name: 'Alice',
  age: 25
};
~~~

2. 函数签名
~~~ts
interface Greet {
	(name: string, num: number): string
}
const greetFun: Greet = (name: string, number: number) => {
	return name + number;
}
~~~

3. 继承接口
~~~ts
interface Animal {
  name: string;
  eat(): void;
}

interface Dog extends Animal {
  bark(): void;
}

const dog: Dog = {
  name: 'Buddy',
  eat() {
    console.log('Eating...');
  },
  bark() {
    console.log('Woof!');
  }
};
~~~

~~~
总结：
implements 主要用于类去实现接口，确保类包含接口中定义的属性和方法。
接口本身可以直接作为类型约束对象、函数等。
接口还可以通过继承（extends）来组合其他接口的结构。
接口的用途非常灵活，除了类的实现之外，完全不需要 implements 关键字也能有效地约束对象、函数、甚至是类的类型结构。
~~~

# Type
在 TypeScript 中，type 是用来定义类型别名的关键字，它允许你给一个类型指定一个新的名称。与接口 (interface) 相似，type 可以用来描述各种类型结构，比如原始类型、联合类型、交叉类型、元组等。
![[../../img/Pasted image 20250303102341.png]]
![[../../img/Pasted image 20250303102353.png]]
![[../../img/Pasted image 20250303102420.png]]
![[../../img/Pasted image 20250303102440.png]]

# type和interface的区别
![[../../img/Pasted image 20250303102936.png]]
![[../../img/Pasted image 20250303102957.png]]![[../../img/Pasted image 20250303103014.png]]
![[../../img/Pasted image 20250303103027.png]]
![[../../img/Pasted image 20250303103054.png]]
