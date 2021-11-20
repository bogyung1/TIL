# CH05. ref: DOM에 이름 달기                                                    
리액트 프로젝트 내부에서 DOM에 이름을 다는 방법: ref(reference) 개념

### 리액트 컴포넌트 안에서는 id를 사용하면 안될까?
- JSX 안에서 DOM에 id를 달면 해당 DOM을 렌더링할 때 그대로 전달된다.
-같은 컴포넌트를 여러번 사용한다고 가정,
- HTML에서 DOM의 id는 유일해야 하는데, 이러한 상황에서는 중복 id를 가진 DOM이 여러개 생기니 잘못된 사용이다.
- ref는 전역적으로 작동하지 않고 컴포넌트 내부에서만 작동하므로 이러한 문제가 발생하지 않는다.

## ref는 어떤 상황에서 사**용하나?
**DOM을 직접적으로 건드려야 할 때** 사용한다. 
함수형 컴포넌트에서 ref를 사용하려면 *Hooks**를 사용해야 한다.
### DOM을 꼭 사용해야 하는 상황
- 특정 input에 포커스 주기
- 스크롤 박스 조작하기
- Canvas 요소에 그림 그리기 등

## ref 사용
### 콜백 함수를 통한 ref 설정
- ref를 달고자 하는 요소에 ref라는 콜백 함수를 props로 전달해 주면 된다. 이 콜백 함수는 ref 값을 파라미터로 전달받는다.
- 함수 내부에서 파라미터로 받은 ref를 컴포넌트의 멤버 변수로 설정해준다.

```js
<input ref={(ref)=> {this.input=ref}} />
```
this.input은 input 요소의 DOM을 가리킨다.

### createRef를 통한 ref 설정
리액트에 내장되어 있는 createRef라는 함수를 사용하는 것이다.
```js
class RefSample extends Component{
    input=React.createRef();

    handleFocus=()=>{
        # DOM에 접근하려면 this.input.current로 조회하면 된다.
        this.input.current.focus();
    }
    render(){
        return(
            <div>
                <input ref={this.input} />
            </div>
        )
    }
}
```

## 컴포넌트에 ref 달기
주로 컴포넌트 내부에 있는 DOM을 컴포넌트 외부에서 사용할 때 쓴다.
```js
<MyComponent
    ref={(ref)=> {this.myComponent=ref}} />
```
이렇게 하면 MyComponent 내부의 메서드 및 멤버 변수에도 접근할 수 있다. 즉, 내부의 ref에도 접근할 수 있다.



