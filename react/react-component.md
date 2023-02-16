# React Component

## 학습 키워드 ✏️
- REST API 와 GraphQL
    - REST API 란 무엇인가
    - GraphQL은 왜 등장했는가?
    - REST API vs GraphQL
- JSON
- DSL(Domain-Specific Language)
- 선언형 프로그래밍
- 명령형 프로그래밍
- SRP(단일 책임 원칙)
- Atomic Design
- React component 와 props


## REST API 와 GraphQL
### 데이터
- 백엔드에서 JSON 형태로 프론트엔드 시스템한테 제공을 해준다.
- 우리는 fetch API 이용해서 기본적인 CRUD(GET/POST/PUT/DELETE)를 할 수 있다. 


### REST API 와 GraphQL
REST API => resource 중심
GraphQL
- Graph 자료구조
- Query(Read), Mutation(Command: Create, Update, delete,)
Subscript(Event)

RESTAPI와 GraphQL의 차이점
만약 어떤 게시물에 정보를 얻고 싶다고 가정하면, 
REST API는 게시물을 얻었는데 그 안에 있던 댓글의 정보까지 같이 얻어오게 된다.
하지만 GraphQL은 Query에서 얻고자 하는 것을 지정할 수 있다.
가령 게시물만 얻을 것인지, 그 밑에 딸려있는 댓글까지 가지고 올 것인지 
그것을 Query에서 지정을 해줄 수 있다.
> 축약:  Rest는 통으로 넘어오고, GraphQL은 내가 필요한 것만 지정해서 가져올 수 있음

## JavaScript Object Notation (JSON)
- 프론트랑 백엔드 사이에서 구조화된 데이터를 표현하기 위한 만들어진 데이터 표준 포맷이다.
- 기본적으로 string이다.
- JSON.stringify -> 스트링을 json
- JSON.parse 문자열을 -> 자바스크립트 객체로

즉, 자바스크립트에는 converter가 이미 있다는 것.

## 선언형 UI, 그리고 DSL(Domain-Specific Language)
F/E는 이 데이터를 사용자가 볼 수 있도록 UI를 구성한다.
순수 자바스크립트에서는 append()하고 좀 복잡하지만, 
리액트는 그럴 필요 없이 선언형 UI를 구성한다.
> 선언형 UI를 구성한다라는 것은, HTML과 유사한 모양의 DSL을 쓴다고 말할 수 있는데,
여기서 DSL은 도메인 특화 언어(특정 도메인에 대한 문제를 풀기 쉽게 하기 위한 언어)이다.

## 컴포넌트 계층 구조

**리액트의 핵심**
1. `Component-Based이다.`
자신의 상태를 캡슐화하고, 그것을 조립하여 만든다.
개별로써의 컴포넌트는 심플해야 한다. 복잡하면 안됨
2. 간단한 컴포넌트를 이용해서, 복잡한 UI를 만든다. ex) 나사 + 엔진 + 등등 = 자동차

### 컴포넌트란?

- 개념적으로 컴포넌트는 JavaScript함수와 유사하다.
- `props`라고 하는 임의의 입력을 받은 후, 리액트 엘리먼트를 반환한다.
- 리액트 엘리먼트는 화면에 어떻게 표시되는 지를 기술하므로, 곧 이 말은 화면이 그려진다는 말.

예시)
```
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

> 그렇다면 컴포넌트를 나누는 기준은?
1. [SRP : 단일 책임 원칙](https://ko.wikipedia.org/wiki/%EB%8B%A8%EC%9D%BC_%EC%B1%85%EC%9E%84_%EC%9B%90%EC%B9%99)
- `모든 컴포넌트는 하나의 책임만 가지고, 캡슐화 해야 한다.`
- 하나의 컴포넌트가 변경되는 이유는 하나여야 한다. 
- 사소한 변경이 있을 시 그에 관련한 것 하나만 고칠 수 있도록.
2. CSS  
- 기본적으로 html이 커지는 경우가 많은데 그런 경우는 적절하게 css selector 기준
3. Design layer
 - 디자인도 트리형태로 되어있음. 그대로 컴포넌트로 활용할 수 았다.
4. information Architecture(JSON Schema 의 영향):  
- 실제로 엄청 많기 쓰게 된다 자연스러운 SRP를 위해서 사실상 강제가 된다. 프론트에서 재가공을
하기도 하고, 백엔드에 다시 요청을 하기도 함

   
작은 컴포넌트 = 부품 

=> 부품을 잘 만들어야 조립하기 쉽다. 더 나아가 조합의 가지수를 폭발적으로 늘릴 수 있다.

Atomic Design는 잘 알고 있는 계층형 구조를 몇가지 카테고리로 묶은 방법

=> 개념적인 것이고, 무조건 다 지킬 필요는 없다. 

### props란
- [Passing Props to a Component](https://beta.reactjs.org/learn/passing-props-to-a-component)
- [Components와 Props](https://ko.reactjs.org/docs/components-and-props.html)


- props는 컴포넌트간의 연결이다.
- props는 읽기 전용이며, 불변해야 하기 때문에 컴포넌트의 자체 props를 수정해서는 안된다.
- 수정하려면 복사해서 사용해야 함.
- 컴포넌트에서 props를 다룰 때는 반드시 순수 함수처럼 동작해야 한다.
- TypeScript를 잘 쓰거나 잘못 쓰게 되는 포인트 중 하나. 적절한 균형점을 잡는 게 중요하다.
- 테스트 코드를 작성하면 재사용성을 평가하기 쉬워짐.
> 순수 함수: 쉽게 말해 입력값을 바꾸려 하지 않고, 항상 동일한 입력값에 대해 동일한 결과를 반환한다.
```javascript
// 순수 함수
function sum(a, b) {
  return a + b;
}

// 자신의 입력값을 변경하기 때문에 순수 함수가 아니다.
function withdraw(account, amount) {
  account.total -= amount;
}
```

## Thinking in react
**Step1: 정적인 것을 먼저 만들어라**
- 데이터를 먼저 보여줄 건지 말건지 정하기.
- 리액트가 컴포넌트 인스턴스를 식별하는 방법으로 `key`가 있기 때문에, `key`는 유니크 해야한다.

```
 const categories =  products.reduce((acc, product) => (
  acc.includes(product.category) ? acc : [...acc, product.category]
), []);
```
리듀스 : 누적된거, 현재아이템
=> 지금 받은 product의 카테고리가, 이미 누적된 값 안에 포함되어있다면
추가안하고, 없다면 추가함

덩어리들을 충분히 잘게 나눌 수 있음.
여러개를 쪼개고, 쪼갠 것들이 어떤 데이터를 취하게 될 것인가를 고민하면서 만든다.

정규표현식 / / g : 글로벌

## utils
- 길게 코드를 쓰고 적절히 자를 수 있는 부분에서 함수로 추출한다.
- 하나씩 함수로 뺀 다음 파일을 만들어도 된다.

필터링 로직 다른 파일로 빼주기
```
const selectProducts = (items:  field: ) => {
  return items.filter(item => item[field] === value)
}
export default select

// utils/selectCategories.tss
const selectCategories = (products) => {
  return products.reduce((acc, product) => {
   return acc.includes(product.category) ? acc : [...acc, product.category]
  }, [])
  
}
```
## 생각해 볼 것
- 카테고리가 이름과 product를 처음부터 갖게 했다면, 훨씬 단순해진다.
- `selectProducts`이런 함수도 필요 없을 수 있다.
이미 선택 된 값이 넘어오기 때문임.

