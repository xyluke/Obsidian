~~~js
this.globalDsv.templateType = 'task.OrderTask/listTimesheetRedo';
this.globalDsv.printMul = true;
function customerPrint(query) {
  console.log(this,query);
  let selection = this.dataVm.selection;
  query.params.ids = selection && selection.map(item => item.id);
  query.needBatch = true;
}

this.globalDsv.customerPrint = customerPrint;
~~~

![[../img/Pasted image 20250317164000.png]]

![[../img/Pasted image 20250317164041.png]]

# vform的data-table按钮问题
onHideOperationButton
~~~js
console.log(buttonConfig);
if (buttonConfig.name == 'createPre') {
    if (row.status.id == 'Finished' || row.status.id == 'Audit') {
        return false;
    } else {
      return true
    }
}
return false;
~~~

onOperationButtonClick
自己要特殊定制代码操作时候
~~~js
// this.getGlobalDsv().handleVformMethods(buttonName,rowIndex, row);
let _that = this.getGlobalDsv();

if (buttonName == 'detail') {
  _that.toDetail(row.id);
}

if (buttonName == 'delete') {
  _that.preTableDataDelete && this.preTableDataDelete(row);
  _that.toDelete(row.id, row.code);
}

if (buttonName == 'createPre') {
  let query = JSON.parse(JSON.stringify(row));

  query.materialId = query.material && query.material.id;
  query.materialName = query.material && query.material.name;
  query.orderProductItemId = null;
  query.taskRedoId = query.id;
  query.taskRedoCode = query.code;
  query.code = null;
  query.id = null;
  query.orderCode = query.workOrderCode;
  query.orderId = query.workOrderId;
  query.subType = null;
  window.axios.post('/task.Timesheet/add', [query]).then(rs =>{
    console.log(rs);
})
}
~~~
