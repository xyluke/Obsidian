interface 声明不管类型的方式
~~~ts
interface MyComponentProps {
  requiredParam: string; // 必需的参数
  optionalParam?: number; // 可选的参数
  [key: string]: any; // 任意其他参数，类型为 any
}

const MyComponent: React.FC<MyComponentProps> = (props) => {
  console.log(props.requiredParam);  // 类型是 string
  console.log(props.optionalParam);  // 类型是 number | undefined
  console.log(props.someOtherParam); // 类型是 any，可以是任意类型

  return <div>{props.requiredParam}</div>;
};
~~~
![[../../img/Pasted image 20250310115157.png]]
![[../../img/Pasted image 20250310115207.png]]


