
~~~js
function greet() {
  console.log('Hello, world!');
    let a = {a: '123'}
    c = () =>{
        console.log(a);
    }
}
let functionStrings = greet.toString();
const methodBody = functionStrings.match(/{([\s\S]*)}/)[1];
console.log(methodBody,functionStrings.match(/{([\s\S]*)}/));
~~~

![[../img/Pasted image 20231211101214.png]]