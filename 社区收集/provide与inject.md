~~~js
provide(){
	return{
		setVformEnable: () => {
			let refs = {
				vformRef: this.$refs.vFormRef,
				vformRef1: this.$refs.vFormRef1
			}
			return refs
		},
		getWidgetList: () => {
			return this.formConf
		},

		CloseCenterDialog: () => {
			this.$emit('toCloseCenterDialog');
		}
	}
}

inject: ['setVformEnable','getWidgetList', 'CloseCenterDialog'],
~~~