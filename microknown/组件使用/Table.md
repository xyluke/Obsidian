# hydra-table
## 仅展示
~~~js
<hydra-column label="物料编码" prop="material.code" />
~~~
## 可编辑
~~~js
<hydra-column-input label="生产数量" prop="totalNum" width="80" />
~~~

## 枚举可编辑
~~~js
<hydra-column-edit-enum 
label="展开类型"
prop="spreadType" 
:data="spreadTypeList"/>

// SpreadTypeList是通过string
/*
export const SpreadTypeEnum = {
NoSpread: StringEnum.create3("NoSpread", "不展开", TagColorConst.Warn),
MainProduct: StringEnum.create3("MainProduct", "仅主产品", TagColorConst.Success),
BomSpread: StringEnum.create3("BomSpread", "按Bom展开", TagColorConst.Info2),
}
export const SpreadTypeList = StringEnum.buildStatusList(SpreadTypeEnum);

需要初始值
*/
spreadTypeList: SpreadTypeList
~~~

## 日期
~~~js
<hydra-column-date-picker label="交货日期" prop="deliveryDate" width="130" />
~~~

