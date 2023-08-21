# 普通
> node-key必须唯一
1. 选中node
2. 取消选中node
~~~js
// 使用的方法 setCheckedKeys([])  setCheckedKeys([node.id])
// 如果是要v-for el-tree 则要像这样遍历 ref
~~~


~~~js
 <el-tree  
	 :data="allDepartList" 
	 :ref="`roleTree${scoped.$index}`" 
	 show-checkbox  
	 check-strictly 
	 @check="(cur,key) => onDepartCheck(cur, key, scoped.row, scoped.$index)" 
	 @check-change="handleClickChange" 
	 node-key="id" 
	 default-expand-all >

	 <span 
		 class="custom-tree-node" 
		 slot-scope="{ data }"  
		 @click="handleClickRowspan(scoped.row, scoped.$index)" >
		<i :class="selectIcon(data.item)" />
		<span>{{data.label}}</span>
	</span>

</el-tree>
~~~