~~~js
 window.axios.post('/task.Order/createFromMrpProducePlan', []).then(rs =>{
    console.log(rs);
})
    window.axios.post('/datax.MetaDynamic/summaryPage', [{
        entityCode: "fd_payable_init",
        filterMap: {},
        fuzzyMap: {supplier_id: value.id},
        isRelation: false,
        itemQuerier:[],
        itemQUeryAllTag: true,
        orderList:[],
        page: 1,
        pageSize: 20,
        rangeMap: {}
    }]).then(rs => {})
~~~