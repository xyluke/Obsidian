~~~js
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';

let FormData = {}

export const onGetMyFlowList = createAsyncThunk("flow/onGetMyFlowList", getMyFlowList);

async function getMyFlowList(data, thunkAPI) {
    // 测试一下这里可不可以直接使用那个dispatch的方法；
    // console.log(thunkAPI());
    // const { setMyProcessList } = thunkAPI;
    await FlowAbleApi.getMyProcessList().then(rs => {
        let asyncData = rs.itemList ? rs.itemList : [];
        thunkAPI.dispatch(setMyProcessList(asyncData));
    })
}

export const flowSlice = createSlice({
    name: "flow",
    initialState: FormData,
    reducers: {
        init() {
            return FormData;
        },
        setTodoList(state, action) {
            console.log("设置待办的人物列表", action.payload);
            let todoList = action.payload;
            return {...state, todoList};
        },
        setMyProcessList(state, action) {
            console.log("设置当前的进程列表");
            let myProcessList = action.payload;
            return {...state, myProcessList};
        },
        setStatus(state, action) {
            console.log("状态设置", action)
            let status = action.payload;
            return { ...state, status};
        },
        setCurrentOrganName(state, action) {
            let currentOrganName = action.payload;
            return {...state, currentOrganName};
        }
    }

})

  

export const  {
    init,
    setTodoList,
    setMyProcessList,
    setStatus,
    setCurrentOrganName
} = flowSlice.actions;
  
export default flowSlice.reducer;
~~~


~~~js
// 使用
import {connect} from "react-redux";
import { onGetMyFlowList, onGetFlowRecordList, setStatus }  from "@/model/flowSlice";
import { setVformData } from "@/model/portal";
import { onStopProcess } from "@/models2/flowSlice";



class FlowableMy extends Component {

	getTodoList = async() => {
        // @ts-ignore;
        const { onGetMyFlowList } = this.props;
        await onGetMyFlowList();
    }

	render(){
		const { isHeader, myProcessList } = this.props;
		return <View>{isHeader}</View>
	}
}





const mapStateToProps = (state) => {
    const { isHeader } = state.portal; // 初始化时候定义的值
    const { myProcessList } = state.flow;
    return {
        isHeader,
        myProcessList
    }
}

const mapDispatchToProps = (dispatch) => ({
    onGetMyFlowList: (action) => dispatch(onGetMyFlowList(action)),
    setVformData: (action) => dispatch(setVformData(action)),
    setStatus: (action) => dispatch(setStatus(action)),
    onGetFlowRecordList: (action) => dispatch(onGetFlowRecordList(action)),
    onStopProcess: (action) => dispatch(onStopProcess(action))
})

export default connect(mapStateToProps, mapDispatchToProps)(FlowableMy);
~~~





# 初始化的models
~~~js
import { configureStore } from "@reduxjs/toolkit";
import counterReducer from './counterSlice';
import portalReducer from './portal';
import flowReducer from './flowSlice';
import wmsReducer from './wmsSlice';
import qualityReducer from "./qualitySlice";
import eamReducer from "./eamSlice";
import formDataReducer from "./formDataSlice"

export default configureStore({
  reducer: {
    counter: counterReducer,
    portal: portalReducer,
    flow: flowReducer,
    wms: wmsReducer,
    quality: qualityReducer,
    eam: eamReducer,
    formData: formDataReducer
  }
})
~~~

注册初始化的models`app.tsx`
~~~js
import store from "./models2";

class App extends Component<Props> {
    render() {
        return (
        <Provider store={store}>
            {this.props.children}
        </Provider>
        )
    }
}
export default App;
~~~


# 使用问题
> moduleA 要使用moduleB的方法

~~~js
// moduleA
import { createSlice } from '@reduxjs/toolkit';
const initData = {
	data1: '初始化数据'
}

export const moduleAslice = createSlice({
	name: 'moduleA',
	initialState: initData,
	reducers:{
		init() {
			return initData
		}
	}
})

export const {
	init
} = moduleAslice.actions;

export default flowSlice.reducer

// moduleB 
import { init as moduleAInit } from "./moduleAslice";
import { createSlice, createAsyncThunk} from '@reduxjs/toolkit';

export const onInitModule = createAsyncThunk('moduleB/onInitModule', initModule);

function initModule(data, thunkApi) {
	// 初始化moduleA
	thunkApi.dispatch(moduleAInit());
	// 初始化moduleB
	thunkApi.dispatch(init());
	return
}

const initDataB = {
	name: 'moduleB的初始数据'
}

export const moduleBslice = createSlice({
	name: 'moduleB',
	initialState: initDataB,
	reducers: {
		init(){
			// B的重置数据
			return initDataB;
		}
	}
})

export const = {
	init
} = moduleBslice.actions;

export default moduleBslice.reducer;
~~~
