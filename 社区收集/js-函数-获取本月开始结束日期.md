
当月第一天
~~~js
function   currentTime() {
      const currentDate = new Date();
      const year = currentDate.getFullYear();//获取当前年
      const month = String(currentDate.getMonth() + 1).padStart(2, "0");//获取当前月
      const firstDay = "01";//日
      return `${year}-${month}-${firstDay}`;
}
~~~


当月最后一天
~~~js
function getLastDayOfNaturalMonth() {//下个月的1号减去1天
  const currentDate = new Date();
  const year = currentDate.getFullYear();//获取当前年
  const month = String(currentDate.getMonth() + 2).padStart(2, "0");//获取当前月
  const firstDay = "01";//1号
  const firstDayOfNextMonth = `${year}-${month}-${firstDay}`;
  const lastDayOfMonth = new Date(new Date(firstDayOfNextMonth).getTime() - 86400000);
  return lastDayOfMonth.toISOString().split("T")[0];
},
~~~

~~~js
function getCurrentDate() {
  const today = new Date();
  const year = today.getFullYear();//获取当前年
  const month = String(today.getMonth() + 1).padStart(2, '0');//获取当前月
  const day = String(today.getDate()).padStart(2, '0');//获取当前日
  return `${year}-${month}-${day}`;
}
~~~

看string有那些方法
> console.log(new String('123')) 可以看到string的方法
> ![[../img/Pasted image 20231020101458.png]]

