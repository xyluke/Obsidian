
store的registerModule注册命名空间
~~~js
store.registerModule(WmsStore, wms);
store.registerModule(EquipmentStore, equipment);
store.registerModule(QualityStore, quality);
store.registerModule(ErpStore, erp);
store.registerModule(VformStore, vform);
~~~


~~~js
// 仓库状态
const state = {
    purchaseOrderHistory: false,
    purchaseArrivalHistory: false,
};
const mutations = {
    set(state, payload) {
        state.purchaseOrderHistory = payload.isHistory;
    }
};
export default {
    // 使该模块成为命名空间模块
    namespaced: true,
    state,
    mutations
}
~~~