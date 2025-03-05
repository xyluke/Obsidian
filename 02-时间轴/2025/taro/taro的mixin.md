~~~ts
import store from "@/model/index";
import { setFormDataInit } from "@/model/formDataSlice";
import extensionFun from "./extensionVform/index";
interface StateType {};
interface PropsType {};

const withMixin = (WrappedComponent) => {
    return class MixinComponent extends WrappedComponent<PropsType, StateType> {
        extentionFun = (funName, value) => {
            // @ts-ignore;
            let entityCode = store.getState().flow.entityCode;
            if (!entityCode || !extensionFun[entityCode]) {
                return;
            }
            let funObj = extensionFun[entityCode].getFunctionByName(this.props.field.options.name);
            if (funObj && funObj[funName] && typeof funObj[funName] === 'function') {
                funObj[funName].call(this, value);
            }
        }
        handleInputCustomEvent(value) {
            // 此处为formModel更改值
            this.syncUpdateFormModel(value);
            // 触发扩展的方法
            this.extentionFun('input', value);
        }
        handleFocusCustomEvent(value) {
            this.extentionFun('focus', value);
        }
        syncUpdateFormModel(value) {
            let { subFormName, subFormItemFlag, rowIndex, field } = this.props
            // @ts-ignore;
            let { originData } = store.getState().formData;
            let propName = field && field.options && field.options.name;
            if(subFormItemFlag) {
                // subFormName: 当前的subform数据  console.log(this.props.subFormName, this.props.rowIndex);
                // rowIndex: 当前行的索引值
                let tempData = originData[subFormName] ? JSON.parse(JSON.stringify(originData[subFormName])) : [{}] ;
                let subFormDataRow = tempData[rowIndex];
                subFormDataRow && (subFormDataRow[propName] = value);
                // @ts-ignore;
                store.dispatch(setFormDataInit({key: subFormName, value: tempData}));
            } else {
                let key = propName;
                // @ts-ignore;
                store.dispatch(setFormDataInit({key, value}));
            }
        }
    }
};

export default withMixin;
~~~