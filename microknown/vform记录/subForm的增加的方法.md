
~~~js
    let itemsRef = this.getWidgetRef("allot_items");

    const dsv = this.getGlobalDsv();
    let fromWare = dsv["from_warehouse"];
    let toWare = dsv["to_warehouse"];

    let rowIndex = this.getRowIndex(newRowId);
    let row = subFormData[rowIndex];
    row.from_area_id = fromWare.defaultArea;
    row.from_position_id = fromWare.defaultPosition;
    row.to_area_id = toWare.defaultArea;
    row.to_position_id = toWare.defaultPosition;
    itemsRef.setSubFormValues(subFormData);
~~~

![[../../img/Pasted image 20241031164320.png]]