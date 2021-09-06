# CH07. 컴포넌트의 라이프사이클 메서드

참고: 라이프사이클 메서드는 클래스형 컴포넌트에서만 사용할 수 있다.

## 라이프사이클 메서드의 이해
라이프사이클은 총 세 가지, 즉 마운트, 업데이트, 언마운트 카테고리로 나눌 수 있다.
- 업데이트: 리렌더링: 컴포넌트 정보를 업데이트
- 마운트: 페이지에 컴포넌트가 나타남
- 언마운트: 페이지에서 컴포넌트가 사라짐

###
마운트: DOM이 생성되고 웹 브라우저상에 나타나는 것을 마운트라고 한다. 
- constructor: 컴포넌트를 새로 만들 때마다 호출되는 클래스 생성자 메서드
- getDerivedStateFromProps: props에 있는 값을 state에 넣을 때 사용하는 메서드
- render: 우리가 준비한 UI를 렌더링하는 메서드
- componentDidMount: 컴포넌트가 웹 브라우저상 나타난 후 호출하는 메서드
###
업데이트: 
- props가 바뀔 때
- state가 바뀔 때
- 부모 컴포넌트가 리렌더링될 때
- this.forceUpdate로 강제로 렌더링을 트리거할 때
###
언마운트: 컴포넌트를 DOM에서 제거하는 것

## 라이프사이클 메서드 살펴보기
###  render
컴포넌트 모양새를 정의한다.<br>

(주의사항)<br>
이 메서드 안에서는 이벤트 설정이 아닌 곳에서 setState를 사용하면 안 되며, 브라우저의 DOM에 접근해서도 안된다. DOM 정보를 가져오거나 state에 변화를 줄 때는 componentDidMount에서 처리해야 한다.

### constructor 메서드
초기 state를 정할 수 있다.

### getDerivedStateFromProps 메서드
props로 받아 온 값을 state에 동기화시키는 용도로 사용하며, 컴포넌트가 마운트될 때와 업데이트될 때 호출된다.

### componentDidMount 메서드
컴포넌트를 만들고, 첫 렌더링을 다 마친 후 실행한다. 이 안에서 자바스크립트 라이브러리 또는 프레임워크의 함수를 호출하거나 이벤트 등록, setTimeout, setInterval, 네트워크 요청 같은 비동기 작업을 처리하면 된다.

### shouldComponentUpdate 메서드
```
shouldComponentUpdate(nextProps, nextState){...}
```
props 또는 state를 변경했을 때, 리렌더링을 시작할지 여부를 지정하는 메서드이다. 이 메서드에서는 반드시 true 값 또는 false 값을 반환해야 한다.

### getSnapshotBeforeUpdate 메서드
render에서 만들어진 결과물이 브라우저에 실제로 반영되기 직전에 호출된다.

### componentDidUpdate 메서드
리렌더링을 완료한 후 실행된다. 업데이트가 끝난 직후이므로, DOM 관련 처리를 해도 무방하다.

### componentWillUnmount 메서드
컴포넌트를 DOM에서 제거할 때 실행한다.componentDidMount에서 등록한 이벤트, 타이머, 직접 생성한 DOM이 있다면 여기서 제거 작업을 해야 한다.

### componentDidCatch 메서드
컴포넌트 렌더링 도중에 에러가 발생했을 때 애플리케이션이 먹통이 되지 않고 오류 UI를 보여 줄 수 있게 해준다. 
```
componentDidCatch(error, info){
    this.setState({
        error: true
    });
    console.log({error, info});
}
```


