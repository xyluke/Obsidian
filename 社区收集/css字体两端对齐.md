![[../img/Pasted image 20231013103427.png]]
> 上诉情况想实现两端字体对齐
> 针对这种排版，因为字数不确定，所以我们不好设置固定距离，但是正好css的样式里有一个**text-align设置内容对齐**，并且有一个是正好设置两端对齐**text-align: justify**

![[../img/Pasted image 20231013103824.png]]

# 解决方案
1. text-align:justify 不处理强制打断的行，也不处理块内的最后一行。
   通俗一点讲，就是**只有一行**显示的时候这个属性是**不起作用**的，或者使用了word-break: break-all;这种**强制换行**的属性，也是**不起作用**的。

2. 如果内容是多于一行的时候，除了最后一行，都是两端对齐的效果。
总结就是（**文字内容要超过一行，最后一行没有两端对齐效果**），但是表单的内容就是一排要两端对齐，咋整嘞？ 各位小伙伴不要着急，我们当然也有很好的解决方法！

****
[css设置文本和内容两端对齐](https://zhuanlan.zhihu.com/p/495014462)
