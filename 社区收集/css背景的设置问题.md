# 示例
~~~css
.body {
	background-image: url('gradient_bg.png');
	background-repeat: repeat-x; /* repeat-y    no-tepeat 不重复 */
	background-position: center center;
	background-size: contain;

	background-attachment: fixed; /* scroll 是否随页面其余部分滚动*/ 
}

~~~

# `background-position`

![[../img/Pasted image 20240523091610.png]]


# `background-size`

| 值          | 描述                                                   |
| ---------- | ---------------------------------------------------- |
| length     | 设置背景图片的宽度和高度                                         |
| percentage | 百分比的背景区域                                             |
| cover      | 此时会保持图像的横纵比完全覆盖背景的定位区域的最小大小。超出容器的部分会被裁剪掉，会导致图片显示不完整。 |
| contain    | 此时会保持图像的纵横比并将图像缩放成适合背景定位区域的最大大小                      |

注意： 
~~~text
 使用background设置背景图，使用background-size: contain 自适应背景定位区域的最大大小，此时如果设置图片高度会导致电脑上正常，显示器上高度不能撑满、或设置高度无效导致图片显示不出来的问题，应该是分辨率兼容性有问题，需要通过设置padding-top百分比将图片高度挤出来，完美解决！
~~~

# `background-clip`

![[../img/Pasted image 20240523094544.png]]

# 简写问题
![[../img/Pasted image 20240523092714.png]]


# 多重背景
![[../img/Pasted image 20240523095814.png]]