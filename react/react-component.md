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

> thinking in react 분석

## REST API 와 GraphQL

데이터. 백엔드에서 제이슨 형태로 프론트엔드 시스템한테 제공을 해준다.
fetch api 이용해서 기본적인 crud, (get, port, put, delete)
rest api => resource 중심

GraphQL
- GraphQL 자료구조
- Query에서 얻고자 하는 것을 지정
- Query(read), Mutation(Command: create, update, deled,)
Subscript(Event)

차이점: rest는 통으로 넘어오고, 그래프큐엘은 내가 필요한 것만 지정해서 가져올 수 있음

## JSON
js lint를 만든사람.
데이터를 교환하기 위해 만듬
프론트랑 백엔드 사이에서
http를 쓰면 string으로 만들어서 보냄 
오브젝트 형태를 띄우길 원함
데이터 포맷이다
기본적으로 스트링이다.

stringify -> 스트링을 json
parse 문자열을 -> 자바스크립트 객체로

converter가 자바스크립트에는 이미 있음

html과 유사한 모양의 DSL을 사용하여 선언형으로 ui를 구성할 수 있다.
dsl => 도메인 특화 언어.

## 컴포넌트 계층 구조
리액트는
선언형
component based

리액트의 강력한 특징. 핵심
- 컴포넌트 베이스다.
- 자신의 상태를 캡슐화하여, 그것을 조립하여 만든다.
- 컴포넌트 하나하나는 복잡하지 않아야함(심플)
- 즉, 간단한 컴포넌트를 이용해서 복잡한 ui를 만든다.
ex) 나사+엔진 등등... = 자동차

컴포넌트를 나누는 기준
SRP: 단일 책임 원칙
-> 모든 컴포넌트는 하나의  책임만 가지고, 캡슐화 해야 한다.
하나의 컴포넌트가 변경되는 이유는 하나여야 한다. 
사소한 변경이 있을 시 그에 관련한 것 하나만 고칠 수 있도록.

css : 기본적으로 html이 커지는 경우가 많은데
그런 경우는 적절하게 css selector 기준

design layer: 디자인도 트리형태로 되어있음. 그대로 컴포넌트로 활용할 수 았다.

information Architecture(JSON Schema 의 영향):  실제로
엄청 많기 쓰게 됨. 자연스러운 SRP를 위해서 사실상 강제가 된다. 프론트에서 재가공을
하기도 하고, 백엔드에 다시 요청을 하기도 함

작은 컴포넌트 = 부품을 만들어서 조립. 조합은 가지수를 폭발적으로 늘릴 수 있는 
가장 전형적인 방법.... 
Atomic Design는 잘 알고 있는 계층형 구조를 몇가지 카테고리로 묶은 방법
=> 개념적인 것이고, 무조건 다 지킬 필요는 없다. 


## thinking in react
step1: 정적인 것을 먼저 만들어라 
데이터를 먼저 보여줄 건지 말건지 정하기.

리액트는 key로 리렌더링을 할지 말지 구별하기때문에, 키는 유니크 해야함
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

## props
컴포넌트간의 연결
길게 코드를 쓰고 적절히 자를 수 있는 부분에서 함수로 추출한다.
하나씩 함수로 뺀 다음 파일을 만들어도 된다.


## utils
필터링 로직 다른 파일로 빼주기
```
const selectProducts = (items:  field: ) => {
  return items.filter(item => item[field] === value)
}
export default select

const selectCategories = (products) => {
  return products.reduce((acc, product) => {
   return acc.includes(product.category) ? acc : [...acc, product.category]
  }, [])
  
}
```

카테고리가 이름과 product를 처음부터 갖게 했다면, 훨씬 단순해진다.
`selectProducts`이런 함수도 필요 없을 수 있다.
이미 선택 된 값이 넘어오기 때문임.

