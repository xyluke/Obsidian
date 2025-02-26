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