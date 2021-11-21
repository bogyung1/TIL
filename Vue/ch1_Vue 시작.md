# CH01. Vue 시작

## Vue.js란?
사용자 인터페이스를 만들기 위한 프로그레시브 프레임워크이다.
- 컴포넌트 기반 프레임워크
<br/>

CDN방식으로 Vue 연결
```html
<!--개발버전, 도움되는 콘솔 경고 포함-->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js "></scirpt>

<!--상용버전, 속도와 용량이 최적화-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

<br />

## 선언적 렌더링

간단한 템플릿 구문을 사용하여 DOM에서 데이터를 선언적으로 렌더링 할 수 있는 시스템

```html
<div id="app">
  {{ message }}
</div>
```

```javascript
var app = new Vue({
  el: '#app',
  data: {
    message: '안녕하세요 Vue!'
  }
})
```
더 이상 HTML과 직접 상호작용할 필요가 없다. Vue는 단일 DOM 요소에 연결되어 DOM 요소를 완전히 제어한다.
<p> 아래와 같이 엘리먼트 속성을 바인딩 할 수 있다.

```html
<div id="main">
    <span v-bind:title="message">
        바인딩 예제</span>
</div>
```
```javascript
var app=new Vue({
    el: '#main',
    data:{
        message: '지금 날짜는'+new Date()+'입니다.'
    }
})
```

#### v-bind
디렉티브(directive) : vue에서 제공하는 특수 속성임을 나타내는 <b>v-</b> 접두어가 붙어있으면 렌더링 된 DOM에 특수한 반응형 동작을 한다.

<br/>

#### v-if
엘리먼트가 표시되는지에 대한 여부를 제어

```html
<p v-if="true">볼 수 있음</p>
```
<br/>

#### v-for
배열의 데이터를 바인딩하여 목록을 표시
```html
<li v-for="item in items">{{list.item}}</li>
<!--이 때, list의 구조는 JSON형태로 되어있다 -->
```
<br/>

#### 사용자 입력 핸들링 v-on
사용자가 앱과 상호 작용할 수 있게 하기 위해 v-on 디렉티브를 사용하여 Vue 인스턴스에서 메소드를 호출하는 이벤트 리스너를 추가한다.
```html
<button v-on:click="callbackFunc">함수호출</button>
```
```javascript
methods:{
    callbackFunc: function(){
        //작성...
    }
}
```
이 때, v-on: 은 @로 대체할 수 있다.

<br />

#### v-model (양방향 바인딩)
입력과 앱 상태를 양방향으로 바인딩하는 v-model 디렉티브 제공
```html
<p>{{ message }}</p>
<input v-model="message">
```
```javascript
var app = new Vue({
  el: '#main',
  data: {
    message: '안녕하세요 Vue!'
  }
})
```


## 컴포넌트 작성법
독립적이며 재사용할 수 있는 컴포넌트로 추상적 개념이다. Vue에서 컴포넌트는 미리 정의된 옵션을 가진 Vue 인스턴스이다. 

<p>등록하는 방법

```js
Vue.component('todo-item',{
  template: '<li>오늘의 할 일</li>'
})
var app=new Vue(...)
```
```html
<ol>
  <todo-item></todo-item>
</ol>
```
부모 영역의 데이터를 자식 컴포넌트에 전달할 수 있어야 한다. prop을 전달받을 수 있도록 수정해본다.
```js
Vue.component('todo-item',{
  props: ['todo'],
  template: '<li>{{todo.text}}</li>'
})

var app = new Vue({
  el: '#main',
  data: {
    //JS 형식으로 데이터가 구성되어 있음
    groceryList: [
      { id: 0, text: 'Vegetables' },
      { id: 1, text: 'Cheese' },
      { id: 2, text: 'Whatever else humans are supposed to eat' }
    ]
  }
})
```
```html
<ol>
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id"
    ></todo-item>
  </ol>
```