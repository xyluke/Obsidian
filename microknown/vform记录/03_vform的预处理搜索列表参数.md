~~~js
function pretreatmentFun(query) {
  if (query.dateRanges && Array.isArray(query.dateRanges)){
      query.startDate = query.dateRanges[0];
      query.endDate = query.dateRanges[1];
  } else {
    let DateUtil = window.DateUtil;
    query.dateRanges = DateUtil.getYesterdayMonthRange();
    query.startDate = query.dateRanges[0];
    query.endDate = query.dateRanges[1];
    console.log(this)
    // let ref = this.getWidgetRef('dateRanges');
    // ref && ref.setValue(query.dateRanges);
    let ref = this.$refs.vFormRef.getWidgetRef('dateRanges');
    ref && ref.setValue(query.dateRanges);
    
  }
  
  if(query.materialIds && Array.isArray(query.materialIds)) {
      query.materialIds = query.materialIds.map(item => item && item.id)
  }
  return query
}
this.globalDsv.preQueryFun = pretreatmentFun;
~~~

~~~js
function pretreatmentFun(query) {
  if(query.type_dict && Array.isArray(query.type_dict)) {
      query.type_dict = {dict: "SaleInvoice", name: "销项发票"}
  }
  return query
}
this.globalDsv.preQueryFun = pretreatmentFun;
~~~


