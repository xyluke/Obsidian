~~~js
onClickDelete() {
if (this.data && Array.isArray(this.data)) {
	 let deleteSet = new Set(this.checkList);  // 用 Set 来提高查找速度
	this.data = this.data.filter(item => !deleteSet.has(item.id));  // 过滤掉需要删除的项
	this.checkList = [];  // 清空已选项
}
},
~~~