# CH01. 리액트 시작

## 왜 리액트인가?
Angularm Backbone.js, Derby.js, Vue.js 등 수많은 프레임워크가 존재한다. 이 프레임워크들은 주로 MVC, MVVM, MVW 아키텍처로 애플리케이션을 구조화한다.
### (공통점)
* Model: 애플리케이션에서 사용하는 데이터를 관리하는 영역
* View: 사용자에게 보이는 부분

###
 프로그램이 사용자에게서 어떤 작업(이벤트)을 받으면 Controller는 모델 데이터를 조회하거나 수정하고, 변경된 사항을 View에 반영한다. 이 방법은 애플리케이션 규모가 커지면 상당히 복잡해지고 관리하지 않으면 성능도 떨어질 수 있다. 이 방식을...
#

### 
**어떤 데이터가 변할때마다 기존 뷰를 날려 버리고 처음부터 새로 렌더링하는 방식** 으로 페이스북 개발팀이 아이디어를 고안하였다.
 최대한 성능을 아끼고 편안한 사용자 경험을 제공하면서 구현하고자 개발한 것: **React**
 + 오직 view만 신경 쓰는 라이브러리

 ## 리액트 특징
 - Virtual DOM
    - DOM(Document Object Model): 객체로 문서 구조를 표현하는 방법, XM이나 HTML로 작성한다.
    - React는 Virtual DOM 방식을 사용하여 DOM 업데이트를 추상화함으로써 DOM 처리 횟수를 최소화하고 효율적으로 진행한다.
    - 실제 DOM에 접근하여 조작하는 대신, 이를 추상화한 자바스크립트 객체를 구성하여 사용한다. 마치 실제 DOM의 가벼운 사본과 비슷하다.
    - (절차)
    1. 데이터를 업데이트하면 전체 UI를 Virtual DOM에 리렌더링
    2. 이전 Virtual DOM에 있던 내용과 현재 내용을 비교
    3. 바뀐 부분만 실제 DOM에 적용

    - 하지만 Virtual DOM을 사용한다고 해서 무조건 빠른 것은 아니다. **지속적으로 데이터가 변화하는 대규모 애플리케이션 구축**할 때 진가가 발휘된다.

- 프레임워크가 아닌 라이브러리
    - 리액트는 오직 View만 담당한다. 기타 기능은 직접 구현하여 사용해야 한다.
    - 라우팅: react-router, Ajax 처리: axios or fetch, 상태 관리: Redux, MobX

#
  
  ## 프로젝트 생성하기
  yarn
  ```
  yarn create react-app react_project
  ```
  npm
  ```
  npm init react-app react_project

  ```
## 리액트 전용 서버 구동
```
yarn start # 또는 npm start
```

