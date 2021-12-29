# Redux

Redux: 기본적으로 <b><u>Javascript</u></b> application들의 state를 관리하는 방법
<p>!주의! redux는 React와 별개로 사용할 수 있다.

```cmd
npm install redux
```

```js
import {createStore} from "redux";
```
- store: state 저장하는 공간 (state: application에서 바뀌는 data)
- reducer: function함수: 데이터를 바꾸고 수정하는 것을 책임진다.
- action: 상태변화가 필요할 때 씀, 반드시 type 필드를 가지고 있어야 한다.

```js
// reducer함수: state 수정, return 하는 것은 your application data
const countModifier=(count=0, action)=>{ //(현재 state, action)state initializing
    console.log(count, action)
    if(action.type==="ADD"){
        return count+1;
    }else if(action.type==="MINUS"){
        return count -1;
    }else{
        return count;
    }
};
const countStore=createStore(countModifier);

//action을 dispatch로 reducer 함수에게 보냄
countStore.dispatch({type:"ADD"});
countStore.dispatch({type:"ADD"});
countStore.dispatch({type:"ADD"});
countStore.dispatch({type:"ADD"});
countStore.dispatch({type:"MINUS"});

console.log(countStore.getState());
```

![ex_screenshot](/images/stateChange.PNG )

순서: store -> reducer 함수 -> action 발생