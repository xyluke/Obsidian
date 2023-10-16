> reactclass类

[多条件判断加类](https://blog.csdn.net/qq_45152044/article/details/125259995)

~~~js

// 普通1
<div>
   <button 
   onClick={this.change.bind(this)}>
	   {this.state.isShow ? '隐藏' : '显示'}
   </button>
   <span className='hidden myspan'></span>
 </div >


//普通2
<div>
   <button 
   onClick={this.change.bind(this)}>
	   {this.state.isShow ? '隐藏' : '显示'}
   </button>
   <span className={'hidden myspan'}></span>
 </div >



//多个1
<div>
	<button onClick={this.change.bind(this)}>
		{this.state.isShow ? '隐藏' : '显示'}
	</button>
	<span 
	className={
		[this.state.isShow ? '':'hidden', 'myspan'].join(' ')
	}>
	</span>
</div >

//多个2
<div>
	<button 
		onClick={this.change.bind(this)}>
		{this.state.isShow ? '隐藏' : '显示'}
	</button>
	<span 
		className={`myspan ${this.state.isShow ? '' : 'hidden'}`}>
	</span>
</div>
~~~

~~~html
<!--多个3-->
<div>
<button 
	onClick={this.change.bind(this)}>
	{this.state.isShow ? '隐藏' : '显示'}
</button>
<span 
	className={this.state.isShow ? "myspan" : "myspan hidden"}></span>
</div >
~~~

四种方法可以实现className的判断加类。  
无需第三方模块依赖：  
1、ES6模板字符串——``  
2、join组成字符串——`Array.join('')`  
3、字符串——`''`

需要第三方依赖：  
classnames——[参考此文](https://wenku.baidu.com/view/4ee0b2d19d3143323968011ca300a6c30c22f1e8.html)