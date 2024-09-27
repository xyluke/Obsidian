
~~~js

import { mapGetters, mapState } from 'vuex';
currentPageCode: (state) => state.currentPageCode

computed: {
	...mapState({
		purchaseArrivalID: (state) => state.purchaseArrivalID,
		pageList : (state) => state.fields,
		currentPageCode: (state) => state.currentPageCode
	})
}
// 建议actived中添加
mounted() {
	let path = this.$route.path;
	this.$store.commit('setPageCode', path);
}

this.arrival.appCode = this.currentPageCode;

~~~
