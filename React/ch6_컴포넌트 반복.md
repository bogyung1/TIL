# CH06. 컴포넌트 반복

## 자바스크립트 배열의 map() 함수
map 함수를 사용하여 반복되는 컴포넌트를 렌더링할 수 있다. map 함수는 파라미터로 전달된 함수를 사용해서 배열 내 각 요소를 원하는 규칙에 따라 변환한 후 그 결과로 새로운 배열을 생성한다.
```
arr.map(callback, [thisArgs])
```
callback:
- currentValue: 현재 처리하고 있는 요소
- index: 현재 처리하고 있는 요소의 index 값
- array: 현재 처리하고 있는 원본 배열
thisArg(선택항목):
- callback 함수 내부에서 사용할 this 레퍼런스

```
const numbers=[1,2,3,4,5];
const result=numbers.map(num=>num*num);
```
## 데이터 배열을 컴포넌트 배열로 변환하기
```
const names=['snowman', 'ice', 'snow', 'wind'];
const nameList=names.map(name=><li>{name}</li>);
return <ul>{nameList}</ul>;
```
## key
리액트에서 key는 컴포넌트 배열을 렌더링했을 때 어떤 원소에 변동이 있었는지 알아내려고 사용한다. key가 없을 때는 Virtual DOM을 비교하는 과정에서 리스트를 순차적으로 비교하면서 변화를 감지한다.

### key 설정
key 값을 설정할 때는 map 함수의 인자로 전달되는 함수 내부에서 컴포넌트 props를 설정하듯이 설정한다. 
- key값은 유일해야 한다.
- 따라서 데이터가 가진 고윳값을 key 값으로 설정해야 한다.

하지만 앞의 코드는 고유 번호가 없다. 이때는 map 함수에 전달되는 콜백 함수의 인수인 index 값을 사용하면 된다.
```
const namesList=names.map((name, index)=> <li key={index}>{name}</li>)
```

## 응용
이제 동적인 배열을 렌더링하는것을 구현해 보자.
### 순서
- 초기 상태 설정하기
- 데이터 추가 기능 구현하기
- 데이터 제거 기능 구현하기

### 초기 상태 설정하기

```
import React, {useState} from 'react';
const IterationSample=()=>{
    const [names, setNames]=useState([
        {id:1, text: 'snowman'},
        {id:2, text: 'ice'},
        {id:3, text: 'snow'}
    ]);
    const [inputText, setInputText]=useState('');
    const [nexId, setNextId]=useState(4); //새로운 항목을 추가할 때 사용할 id

    const namesList=names.map(name=> 
        <li key={name.id}>{name.text}</li>);
        return <ul>{nameList}</ul>;
};

export default IterationSample;

```
### 데이터 추가 기능 구현
```
const onChange=e=> setInputText(e.target.value);

const onClick=()=>{
    const nextNames=names.concat({
        id: nextId,
        text: inputText
    });
    setNextId(nextId+1);
    setNames(nextNames);
    setInputText('');
}
..

<input value={inputText} onChange={onChange} />
<button onClick={onClick}>추가</button>
<ul>{namesList}</ul>
```
push: 기존 배열 자체를 변경해준다.<p>
concat 함수: 새로운 배열을 만들어준다.<p>
리액트에서 상태를 업데이트할 때는 기존 상태를 그대로 두면서 새로운 값을 상태로 설정해야 한다. 이를 **불변성 유지**라고 한다. 불변성 유지를 해 주어야 나중에 리액트 컴포넌트의 성능을 최적화할 수 있다.

### 제거
```
const onRemove=id=>{
    const nextNames=names.filter(name=>name.id!==id);
    setNames(nextNames);
};
...
 <li key={name.id} onDoubleClick={()=>
    onRemove(name.id)}>{name.text}</li>);
```
불변성을 유지하면서 배열의 특정 항목을 지울 때는 배열의 내장 함수 filter를 이용한다.