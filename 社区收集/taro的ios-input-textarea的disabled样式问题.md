> 出现安卓disabled正常 ios不正常

> 调试过程中出现直接给他样式不生效如下：

~~~css
.at-input input[disabled],
.at-input input:disabled{
    color:  #000;
    opacity: 1;
    -webkit-text-fill-color:  #000;
}


.at-textarea__textarea textarea[disabled],
.at-textarea__textarea textarea:disabled{
	color: #000 !important;
	-webkit-text-fill-color: #000 !important;
	-webkit-opacity: 1 !important;
	background-color: transparent!important;
	resize: none;
}
/*发现他一直引入的都是 用户样式表*/
~~~
![[../img/Pasted image 20231201105606.png]]

>最后更改成功input和textarea的disabled的状态是用他们的className来设置css才成功的。
![[../img/Pasted image 20231201105655.png]]

![[../img/Pasted image 20231201105746.png]]

~~~css
.weui-input:disabled{
    color:  #000;
    opacity: 1;
    -webkit-text-fill-color:  #000;
}

.taro-textarea:disabled,
.taro-textarea[disabled] {
	color: #000 !important;
	-webkit-text-fill-color: #000 !important;
	-webkit-opacity: 1 !important;
	background-color: transparent!important;
	resize: none;
}
~~~