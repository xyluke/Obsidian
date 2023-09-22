# hydra-web 出现这个问题
![[img/Pasted image 20230824092612.png]]

# 原因

大概意思就是`vue`在数据变化时，自动插入节点，但是插入节点之前的那个节点已经不在了(或者是还在但是有错误)；

# 解决方法

1. v-if 变成v-show让它在改变的时候不至于删除
2. template的v-if 在包一层
3. 添加key，使用`:key="Math.random()"`  或者 `:key="new Date().getTime()"` 让key动态，更新组件
4. 查看绑定值是否有错误，v-model的绑定值不能是undefined