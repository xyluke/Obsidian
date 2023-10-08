> css有时浮动清除不了

# 常用的清除浮动的方法
**1. 利用`overflow:hidden`清除浮动**
   当父元素应用了`overflow:hidden`就会创建一个新的BFC。让父元素能够正确的包含浮动元素。
~~~css
<div class="father"> 
	<div class="child">子元素 1</div> 
	<div class="child">子元素 2</div> 
	<!-- 更多子元素 --> 
</div>
.father { overflow: hidden; /* 可选：设置父元素的高度 */ } 
.child { float: left; /* 其他样式 */ }
~~~
需要注意的是，`overflow: hidden` 的特性之一是会隐藏溢出的内容。因此，如果父元素内部有内容超出了父元素的大小，这些溢出的内容将被隐藏起来。在某些情况下，可能会不符合预期要求。
   
**2. 利用`display: flow-root`清除浮动**
   `display: flow-root` 是 CSS 中的一个新特性，会建立一个包含块。将所有的浮动元素包含在内部。使得包含块具备BFC特性。添加在父元素上面
~~~css
<div class="father"> 
	<div class="child">子元素 1</div> 
	<div class="child">子元素 2</div> 
	<!-- 更多子元素 --> 
</div>
.father { display: flow-root; } 
.child { float: left; /* 其他样式 */ }
~~~
   
**3. 利用`clear: both`清除浮动**
   (当父元素的高度不足以容纳浮动元素时，将无法起作用。可以尝试给父元素添加一个clearfix类)
   ~~~css
   <div class="father clearfix"> 
	   <div class="child">子元素 1</div> 
	   <div class="child">子元素 2</div> 
	   <!-- 更多子元素 --> 
   </div>
   .clearfix::after { 
	   content: ""; 
	   display: table; 
	   clear: both; 
   }
 ~~~
 