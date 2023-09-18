9-16
![[../img/Pasted image 20230916104306.png]]
![[../img/Pasted image 20230916104311.png]]
改操作的fixed 卡死。

**解决**
原因: tableColumns 计算有问题。当操作fixed为left的情况他没有考虑进去。起始的$index在代码里面是按照代码组件的先后顺序来的。虽然操作为left但是顺序还是2，实际的顺序应该是3这样就会导致计算出undefined的索引值最终undefined.属性死机。
![[../img/Pasted image 20230916114528.png]]