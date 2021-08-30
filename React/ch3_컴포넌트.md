# CH03. 컴포넌트(Component)

컴포넌트를 선언하는 방식은 두 가지이다. 하나는 함수형 컴포넌트, 또 하나는 클래스형 컴포넌트이다. 

## 클래스형 컴포넌트

```
import React, {Component} from 'react';
class App extends Component{
    render(){
        const name='react';
        return <div className="react">{name}</div>;
    }
}
extends default App;
```
- 클래스형 컴포넌트의 경우, state 기능 및 라이프사이클 기능 사용 가능
- 임의 메서드 정의 가능
- render 함수가 꼭 있어야 함(그 안에서 보여 주어야 할 JSX를 반환)

## 함수형 컴포넌트
```
import React from 'react';
const MyComponent=()=>{
    return <div>나의 새롭고 멋진 컴포넌트</div>;
};
export default MyComponent;
```
함수를 작성할 때 function 키워드를 사용하는 대신에 ()=>{}를 사용하여 함수를 만들어 준다.(ES6에 도입)
### (장점)
- 클래스형 컴포넌트보다 선언하기가 훨씬 편리함
- 메모리 자원도 덜 사용함
- 프로젝트를 완성하여 빌드한 후 배포할 때도 함수형 컴포넌트를 사용하는 것이 결과물의 파일 크기가 더 작음
### (단점)
- state와 라이프사이클 API의 사용이 불가능 (리액트 v16.8 이후 Hooks 기능으로 해결)

### 모듈 내보내기 및 불러오기
```
export default MyComponent;
```
이 파일을 import할 때, 위에 선언한 MyComponent 클래스를 불러오도록 설정한다.
```
import MyComponent from '컴포넌트 위치';
...
return <MyComponent/>;
```

## Props
props는 properties를 줄인 표현, 컴포넌트 속성을 설정할 때 사용하는 요소이다.
### JSX 내부에서 props 렌더링
```
const MyComponent = props =>{
    return <div>Hello, My name is {props.name}입니다.</div>
}
```
### 컴포넌트를 사용할 때 props 값 지정하기
```
return <MyComponent name="React" />;
```
### defaultProps
```
MyComponent.defaultProps={
    name: 'default name'
};
```
### 태그 사이의 내용을 보여주는 children
```
return <MyComponent>리액트</MyComponent>;
```
```
<div>
children값은 {props.children}입니다. 
</div>
```
### 비구조화 할당 문법을 통해 props 내부 값 추출하기
위에서 언급한것과 같이 props.name, props.children과 같이 props. 라는 키워드를 붙여주고 있다. 이 방법을 비구조화 할당 문법을 사용해 내부 값을 바로 추출하는 방법을 소개하겠다.(=구조 분해 문법)
```
const {name, children}=props;
...
<div>
 Hello My name is {name}. 
 children name is {children}.
</div>
```
아래와 방법으로도 사용할 수 있다.
```
const MyComponent=({name, children})=>{
    ...
    {name}, {children}
}
```
### propTypes를 통한 props 검증
필수 props를 지정하거나 타입을 지정할 때 사용한다.
```
import PropTypes from 'prop-types;
...
MyComponent.propTypes={
    name: PropTypes.string
};

```
### isRequired를 사용하여 필수 propTypes 설정
propTypes를 지정하지 않았을 때 경고 메시지를 띄워 주는 작업이다.
```
MyComponent.propTypes={
    name: PropTypes.string.isRequired
};
```
### 클래스형 컴포넌트에서 props 사용하기
```
render(){
    const {name, children}=this.props;
    ...
    {name}, {children}
}
```

## State
리액트에서 state는 컴포넌트 내부에서 바뀔 수 있는 값을 의미한다.
- props는 컴포넌트가 사용되는 과정에서 부모 컴포넌트가 설정하는 값이며, 컴포넌트 자신은 해당 props를 읽기 전용으로 사용할 수 있다.
- props를 바꾸려면 부모 컴포넌트에서 바꾸어 주어야 한다.
- class형: state, 함수형: useState

### 클래스형 컴포넌트
```
#생성자 메서드
constructor(props){
    super(props); #상속받고 있는 리액트의 Component가 지닌 생성자 함수 호출
    this.state={
        number:0;
    };
}
render(){
    const {number}=this.state;
    return{
        ...
        this.setState({number:number+1});
    }
}
```
### state를 constructor에서 꺼내기
```
state={
    number:0,
    fixedNumber:0
};
render(){
    const {number, fixedNumber}=this.state;
    return (...);
}
```
### this.setState에 객체 대신 함수 인자 전달하기
this.setState를 사용하여 state값을 업데이트할 때 상태가 비동기적으로 업데이트된다.
### (해결)
this.setState를 사용할 때 객체 대신에 함수를 인자로 넣어준다.
```
#prevState: 기존 상태, props: 현재 지니고 있는 props
this.setState((prevState, props)=>{
    return{
        ...
        <button
            onClick={()=>{
                this.setState(prevState=>{
                    return {
                        number: prevState.number+1;
                    };
                });
                //함수에서 바로 객체를 반환한다는 의미
                this.setStte(prevState=>({
                    number: prevState.number+1
                }));
            }}
        >+1</button>
    }
})
```
### this.setState가 끝난 후 특정 작업 실행하기
setState의 두 번째 파라미터로 콜백 함수를 등록하여 작업 처리한다.
```
this.setState(
    {
        number: number+1
    },
    ()=>{
        console.log('방금 setState가 호출되었습니다.');
        console.log(this.state);
    }
);
```
### 함수형 컴포넌트에서 useState 사용하기
```
#useState 함수의 인자에는 상태의 초기값을 넣어준다.
const [message, setMessag]=useState('');
```
첫 번째 원소: 현재 상태, 두번째 원소: 상태를 바꾸어주는 함수, 이를 세터함수라고 한다.

더 자세한 내용은 8장에서 다루도록 하겠다.



