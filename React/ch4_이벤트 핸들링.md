# CH04. 이벤트 핸들링
사용자가 웹 브라우저에서 DOM 요소들과 상호 작용하는 것을 이벤트라고 한다.

### 이벤트 사용할 때 주의사항
- 이벤트 이름은 카멜 표기법으로 작성한다.
- 함수 형태의 값을 전달한다.
- DOM 요소에만 이벤트를 설정할 수 있다.(즉, 우리가 직접 만든 컴포넌트에는 이벤트를 자체적으로 설정할 수 없다. 하지만, 전달받은 props를 컴포넌트 내부의 DOM 이벤트로 설정할 수는 있다.)

## 이벤트 핸들링 연습

### onChange 이벤트 핸들링
```js
<input
    type="text"
    name="message"
    placeholder="아무거나 입력해 보세요"
    onChange={
        (e)=>{
            console.log(e.target.value);
        }
    }>
```
e: SyntheticEvent로 웹 브라우저의 네이티브 이벤트를 감싸는 객체

```js
<input
    type="text"
    name="message"
    placeholder="아무거나 입력해 보세요"
    value={this.state.message}
    onChange={
        (e)=>{
            this.setState({
                message: e.target.value
            })
        }
    }>
```

### 임의 메서드 만들기

앞에서 언급한 주의사항 중에 "함수 형태의 값을 전달한다"라고 배웠다. 그렇기에 이벤트를 처리할 때 렌더링을 하는 동시에 함수를 만들어서 전달해주겠다. 성능은 별 차이가 없지만, 가독성은 훨씬 높다.

(함수형 컴포넌트로 구현해 보기)
```js
const [username, setUsername]=useState('');
const [message, setMessage]=useState('');
const onChangeUsername=e=> setUsername(e.target.value);
const onChangeMessage=e=> setMessage(e.target.value);

const onClick=()=>{
    alert(username+ ': '+message);
    setUsername('');
    setMessage('');
};
const onKeyPress=e=>{
    if(e.key==='Enter'){
        onClick();
    }
}
```
이 코드는 input의 개수가 많아지면 e.target.name을 활용하는 것이 더 좋다.
이번에는 useState를 통해 사용하는 상태에 문자열이 아닌 객체를 넣어 보겠다.
```js
const [form, setForm]=useState({
    username: '',
    message: ''
});
const {username, message}=form;
const onChange=e=>{
    const nextForm={
        ...form, //기존의 form 내용을 이 자리에 복사한 뒤
        [e.target.name]: e.target.value //원하는 값을 덮어 씌우기
    };
    setForm(nextForm);
}
```
이렇게 input값들이 들어 있는 form 객체를 사용하면 된다.
