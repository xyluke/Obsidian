# 页面代码
~~~html
<template>
<span>
    <el-button icon="el-icon-arrow-left" size="mini" @click="handlePrev" class="pageBtn" />
    <el-button icon="el-icon-arrow-right" size="mini" @click="handleNext" class="pageBtn" />
    <span style="margin-left: 10px">第{{curItem+1}}条 / 共{{dataTotal}}</span>
</span>
</template>

<script>
import { PageReturnDto, PageDto } from "@/ux/page";

export default {
    name: "PaginationButtons",

    props: {
        curId: {
            type: String,
            required: true,
        },

        functionMethod: {
            type: Function,
            required: true,
        },
    },

    data() {
        return {
            query: new PageDto(),
            curIndex: 0,
            curItem: 0,
            dataTotal: 0,
            List: new PageReturnDto(),
        }
    },


    async activated() {
        this.query = this.$store.state.pagedata;

        // @todo 待优化，1) 查找ID这些逻辑不应该在组建内部有如此多的混合逻辑，可以考虑抽象出查找函数
        // @todo 2) 分页数据并不应该每次点击下一次就进行一次查找（比如每次查找了20条数据，那么就应该利用这缓存的数据）
        // @todo 3) 每次点击下一次的时候，总数等都可能变化
        this.List = await this.functionMethod(this.query);
        const items = this.List.itemList;
        if (items && items.length > 0) {
            this.curIndex = items.findIndex(item => item.copySendTaskId === this.curId || item.id === this.curId || item.taskId === this.curId);//这里应为流程数据里面是taskId
            //针对于所有任务，我的任务添加type
            if (this.query.taskType === 'All' || this.query.taskType === 'myPross') {
                this.curIndex = items.findIndex(item => item.procInsId === this.curId);
            }

            this.dataTotal = this.List.total;
            this.curItem = (this.query.page - 1) * (this.query.pageSize) + this.curIndex;
        }
    },


    methods: {
        async handlePrev() {
            if (this.curIndex <= 0 && this.query.page > 1) {
                this.query.page--;
                this.curIndex = this.query.pageSize;
            }

            this.List = await this.functionMethod(this.query);
            const prevItem = this.curItem - 1;
            if (prevItem <= -1 && this.query.page <= 1) {
                this.$message.warning("已经是第一条了！");
                return;
            }

            this.curItem--;
            this.curIndex--;
            this.changePage()
        },

        async handleNext() {
            if (this.curIndex >= this.query.pageSize - 1) {
                this.query.page++;
                this.curIndex = -1;
            }

            this.List = await this.functionMethod(this.query);
            const nextItem = this.curItem + 1;
            if (nextItem >= this.dataTotal) {
                this.$message.warning("已经是最后一条了！");
                return;
            }

            this.curIndex++;
            this.curItem++;
            this.changePage();
        },

        changePage() {
            let param = this.List.itemList[this.curIndex];
            console.log(param,"param");
            this.curId = param.id ? param.id : param.taskId;
            this.$emit('changePage', this.curId, param);
        }
    },
};
</script>


<style scoped>
.pageBtn {
    margin-left: 10px;
    height: 32px;
    width: 52px;
    font-size: 16px;
}
</style>

~~~

# 参数
dataTotal：总条数
curIndex: 当前页索引
curItem: 当前条数
query: 参数源
List：functionMethod的返回值
curId: 接收的当前页的id

# 方法
## 前进 handlePrev
~~~js
async handlePrev() {
	if (this.curIndex <= 0 && this.query.page > 1) {
		this.query.page--;
		this.curIndex = this.query.pageSize;
	}

	this.List = await this.functionMethod(this.query);
	const prevItem = this.curItem - 1;
	if (prevItem <= -1 && this.query.page <= 1) {
		this.$message.warning("已经是第一条了！");
		return;
	}

	this.curItem--;
	this.curIndex--;
	this.changePage()
}
~~~


后退
handleNext
nextItem

更改页面数据的方法
changePage

这里会从store中存储的pagedata，拿到query。

参数functionMethod 是请求列表页的Api


# 快速解读
就是把列表页的query存到store中，然后把id 传入到这个组件，同时上 下 把上一条、下一条的id传回去，然后调用详情页的接口更新数据。


