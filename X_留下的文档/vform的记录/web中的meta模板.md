
![[../../img/Pasted image 20241204115120.png]]
v0 就是测试的默认模板
v1 通用的列表页
v2 通用的列表页
dsv 第三方，能够配置数据源然后根据数据源得到列表数据或者详情数据

# 内置方法
vform中的内置配置的方法都要放在externalVform的扩展方法中
~~~js
export default class InvoiceExtend {
    static getAllFunction() {
        return [
            { code: "supplier_id", onChange: funToString(changeSupplierId)},
            { code: "purchase_arrival_id", /* onChange: "", */ },
            { code: "sale_order_id", /* onChange: "", */ },
            { code: "flow-save", onClick: funToString(saveOnClick) },
            { code: "total_price", onChange: funToString(total_priceChange) },
            { code: "tax_rate", onChange: funToString(tax_rateChange) },
            // { code: 'type_dict', onChange: type_dictChange },
            { code: 'type_dict', onChange: funToString(changeItemType) },
            { code: 'order_id', onChange: funToString(changeOrderId) },
            { code: 'init_id', onChange: funToString(changeInitId)},
            { code: 'fd_invoice_item', onSubFormRowAdd: funToString(subFormRowAdd)}
        ];
    }


    static globalExternal() {
        return {};
    }


    static getOnFormMounted() {
        return funToString(formMounted);
    }
}
~~~