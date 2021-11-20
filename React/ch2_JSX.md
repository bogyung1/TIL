# CH02. JSX

React 를 불러와 사용할 수 있게 해주는 코드
```js
import React from 'react';
```

JSX는 자바스크림트의 확장 문법, XML과 매우 비슷하다.
이러 형식으로 작성한 코드는 브라우저에서 실행되기 전에 코드가 번들링되는 과정에서 *바벨*을 사용하여 *일반 자바스크립트 형태*의 코드로 변환된다.

(예시)
```js
function App(){
    return (
        <div>
        Today <b>I learned</b>
        </div>
    );
}
```
이 코드는
```js
function App(){
    return React.createElement("div", null, "Today ", React.createElement("b", null, "I learned"));
}
```
이렇게 변환된다.

그렇다면 JSX도 자바스크립트 문법이라고 할 수 있는가? 
리액트로 개발할 때 사용되므로 공식적인 자바스크립트 문법은 아니다.

- ### 장점
    - 보기 쉽고 익숙하다. (가독성)
    - 높은 활용도: HTML 태그 사용 가능

## 문법
- 컴포넌트에 여러 요소가 있다면 반드시 **부모 요소** 하나로 감싸야 한다. 
    - why? Virtual DOM에서 컴포넌트 변화를 감지해낼 때 효율적으로 비교할 수 있도록 컴포넌트 내부는 하나의 DOM 트리 구조로 이루어져야 한다는 규칙이 있기 때문이다.
    - 감싸는 요소:
    ```html
    <div>, <Fragment>, <> 등
    ```
- 자바스크립트 표현
    - JSX 내부에서 코드를 { }로 감싸면 된다.
    ```js
    const name='React';
    (중략)
    <h1>{name}Hello!</h1>
    ```
- 조건부 연산자
    ```js
    {name === 'React' ? (<h1>This is React.</h1>): (<h2>This is not React.'</h2>)}
    ```
    ```js
    {name === 'React' && (<h1>This is React.</h1>)}
    ```
- ESLint, Prettier 적용
    - ESLint: 문법 검사 도구
    - Prettier: 코드 스타일링 자동 정리

