# Hooks

함수형 컴포넌트에서 상태 관리를 할 수 있는 useState, 렌더링 직후 작업을 설정하는 useEffect 등의 기능을 제공한다.

## useState
<hr/>

```js
import React, {useState} from 'react';
```
### useState 여러번 사용하기
컴포넌트에서 관리해야 할 상태가 여러 개라면 useState를 여러번 사용한다.
```js
const [name, setName]=useState('');
const [nickname, setNickname]=useState('');

const onChangeName=e=>{
    setNName(e.target.value);
}
const onChangeNickname=e=>{
    setNickname(e.target.value);
}
...

<input value={name} onChange={onChangeName}>
<input value={nickname} onChange={onChangeNickName}>
```
## useEffect
<hr/>
useEffect는 리액트 컴포넌트가 렌더링될 때마다 특정 작업을 수행하도록 설정할 수 있는 Hook이다.

```js
useEffect(()=>{
    console.log('렌더링이 완료되었습니다.);
    console.log({
        name, 
        nickname
    });
});
```
- 마운트될 때만 실행하고 싶을 때
    - 업데이트될 때는 실행하지 않으려면 함수의 두 번째 파라미터로 **비어있는 배열**을 넣어주면 된다.
    ```js
    useEffect(()=>{
        console.log('마운트 될 때만 실행');
    },[]);
- 특정 값이 업데이트될 때만 실행하고 싶을 때
    - 두 번째 파라미터로 전달되는 배열 안에 검사하고 싶은 값을 넣어주면 된다.
    ```js
     useEffect(()=>{
        console.log(name);
    },[name]);

## useReducer
<hr/>
useReducer는 useState보다 더 다양한 컴포넌트 상황에 따라 다양한 상태를 다른 값으로 업데이트 주고 싶을 때 사용하는 Hook이다.<p>
리듀서는 <i>현재 상태, 그리고 업데이트를 위해 필요한 정보를 담은 액션 값을 전달받아 새로운 상태를 반환하는 함수이다.</i> 리듀서 함수에서 새로운 상태를 만들 때는 반드시 <b>불변성</b>을 지켜 주어야 한다.

```js
function reducer(state, action){
    switch(action.type){
        case: 'INCREMENT':
            return {value: state.value+1};
        case: 'DECREMENT':
            return {value: state.value-1};
        default:
            return state;
    }
}

...

const Counter=()=>{
    const [state, dispatch]=useReducer(reducer, {value:0});

    return(
        <div>
            <p>
                현재 카운터 값은 {state.value}입니다.
            </p>
            <button onClick={()=> dispatch({type: 'INCREMENT'})}>+1</button>
            <button onClick={()=> dispatch({type: 'DECREMENT'})}>-1</button>

        </div>
    )
}
```
useReducer의 첫 번째 파라미터에는 리듀서 함수를 넣고, 두번 째 파라미터에는 해당 리듀서의 기본값을 넣어준다.
- state: 현재 가리키고 있는 상태
- dispatch: 액션을 발생시키는 함수 / ex. dispatch(action)
- useReducer의 가장 큰 장점은 컴포넌트 업데이트 로직을 컴포넌트 바깥으로 뺄 수 있다는 점이다.

### 인풋 상태 관리하기
```js
function reducer(state, action){
    return {
        ...state,
        [action.name]: action.value;
    };
}
...
const Info=()=>{
    cosnt [state, dispatch]=useReducer(reducer. {
        name:'',
        nickname:''
    });
    
    const {name, nickname}=state;\

    const onChange=e=>{
        dispatch(e.target);
    }
}
```
useReducer에서의 액션은 그 어떤 값도 사용 가능하다. 이번에는 이벤트 객체가 지니고 있는 e.target 값 자체를 액션 값으로 사용했다.

## useMemo
<hr/>
useMemo를 사용하면 함수형 컴포넌트 내부에서 발생하는 <b>연산</b>을 최적화할 수 있다. 

```js
//짧은 예시
const avg=useMemo(()=> getAverage(list), [list]);
```
렌더링하는 과정에서 특정 값이 바뀌었을 때만 연산을 실행하고, <i>원하는 값이 바뀌지 않았다면 이전에 연산했던 결과를 다시 사용하는 방식이다.</i>

## useCallback
<hr/>
useMemo와 상당히 비슷한 함수이다. 주로 렌더링 성능을 최적화해야 하는 상황에서 사용한다. 이 Hook을 사용하면 만들어 놨던 함수를 재사용할 수 있다.

```js
const onChange=useCallback(e=>{
    setNumber(e.target.value);
},[]); //컴포넌트가 처음 렌더링 될때만 함수 생성

const onInsert=useCallback(()=>{
    const nextList=list.concat(parseInt(number));
    setList(nextList);
    setNumber('');
},[number, list]); //number or list가 바뀌었을 때만 함수 생성
```
useCallback의 첫 번째 파라미터에는 <b>생성하고 싶은 함수</b>를 넣고, 두 번째 파라미터에는 <b>배열</b>을 넣으면 된다. 이 배열에는 어떤 값이 바뀌었을 때 함수를 새로 생성해야 하는지 명시해야 한다.

## useRef
<hr/>
useRef Hook은 함수형 컴포넌트에서 ref를 쉽게 사용할 수 있도록 해준다.

```js
// 버튼을 눌렀을 때 포커스가 인풋 쪽으로 넘어가도록 코드 작성
const inputEl=useRef(null);
...
const onInsert=useCallback(()=>{
    ...
    inputEl.current.focus();
}, [number, list]);

...
<input value={number} onChange={onChange} ref={inputEl} />

```
## 커스텀 Hooks 만들기
<hr />
여러 컴포넌트에서 비슷한 기능을 공유할 경우. 이를 우리만의 Hook으로 작성하여 로직을 재사용할 수 있다.

```js
function default function useInputs(initialForm){
    const [state, dispatch]=useReducer(reducer, initialForm);
    const onChange=e=>{
        dispatch(e.target);
    };
    return [state, onChange];
}
```
이렇게 커스텀 Hooks를 만들어서 사용했던 것처럼, 다른 개발자가 만든 
Hooks도 라이브러리로 설치하여 사용할 수 있다.
