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
REST API 
- resource 중심

GraphQL
- Graph 자료구조
- Query(Read), Mutation(Command: Create, Update, delete,), Subscript(Event)

**RESTAPI와 GraphQL의 차이점**

만약 어떤 게시물에 정보를 얻고 싶다고 가정하면, 
REST API는 게시물을 얻었는데 그 안에 있던 댓글의 정보까지 같이 얻어오게 된다.
하지만 GraphQL은 Query에서 얻고자 하는 것을 지정할 수 있다.
가령 게시물만 얻을 것인지, 그 밑에 딸려있는 댓글까지 가지고 올 것인지 
그것을 Query에서 지정을 해줄 수 있다.
> 축약:  Rest는 통으로 넘어오고, GraphQL은 내가 필요한 것만 지정해서 가져올 수 있음

## JavaScript Object Notation (JSON)
- 프론트랑 백엔드 사이에서 구조화된 데이터를 표현하기 위한 만들어진 데이터 표준 포맷이다.
- 기본적으로 string이다.
- JSON.stringify -> string을 json
- JSON.parse 문자열을 -> 자바스크립트 객체로

> 즉, 자바스크립트에는 converter가 이미 있다는 것.

## 선언형 UI, 그리고 DSL(Domain-Specific Language)
F/E는 이 데이터를 사용자가 볼 수 있도록 UI를 구성한다.
순수 자바스크립트에서는 append()하고 좀 복잡하지만, 
리액트는 그럴 필요 없이 선언형 UI를 구성한다.
> 선언형 UI를 구성한다라는 것은, HTML과 유사한 모양의 DSL을 쓴다고 말할 수 있는데,
여기서 DSL은 도메인 특화 언어(특정 도메인에 대한 문제를 풀기 쉽게 하기 위한 언어)이다.


## 리액트의 핵심
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

### 그렇다면 컴포넌트를 나누는 기준은?
1. 단일 책임 원칙 (Single Responsibility Principle: SRP)
> SOLID의 원칙 중 하나로, 객체 지향 프로그래밍의 유지보수성과 확장성에 도움이 되는 전략이라고 한다.
> 모든 함수/모듈/컴포넌트는 정확히 한 가지 작업을 수행해야 한다는 원칙인데,
> 그 원칙을 잘 지키기 위해서는 다음과 같은 작업을 수행하면 된다.
- 너무 많은 작업을 수행하는 큰 컴포넌트를 더 작은 컴포넌트로 나눈다.
- 주요 컴포넌트 기능과 관련 없는 코드를 별도의 유틸리티 함수로 추출
- 주요 컴포넌트 기능과 관련 없는 코드를 별도의 유틸리티 함수로 추출
- 연결된 기능을 커스텀 훅으로 캡슐화
- 하나의 컴포넌트가 변경되는 이유는 하나여야 한다. (사소한 변경이 있을 시 그에 관련한 것 하나만 고칠 수 있도록.)

글로 적으면 잘 와닿지 않아서 예시를 보니 이해가 쉬웠다.
> [React에 SOLID 원칙 적용하기](https://konstantinlebedev.com/solid-in-react/)
 
>[SOLID에 기초한 코드 작성법](https://velog.io/@huurray/SOLID-%EC%9B%90%EC%B9%99%EC%97%90-%EA%B8%B0%EC%B4%88%ED%95%9C-React-%EC%BD%94%EB%93%9C-%EC%9E%91%EC%84%B1%EB%B2%95)

2. CSS  
- 기본적으로 html이 커지는 경우가 많은데 그런 경우는 적절하게 css selector 기준
3. Design layer
 - 디자인도 트리형태로 되어있음. 그대로 컴포넌트로 활용할 수 았다.
4. information Architecture(JSON Schema 의 영향):  
- 실제로 엄청 많기 쓰게 된다 자연스러운 SRP를 위해서 사실상 강제가 된다. 프론트에서 재가공을
하기도 하고, 백엔드에 다시 요청을 하기도 함


### 컴포넌트 계층 구조 - 아토믹 디자인
작은 컴포넌트를 부품으로 생각해보자,
부품을 잘 만들어야 조립하기 쉽고, 더 나아가 조합의 가지수를 폭발적으로 늘릴 수 있다.

### Atomic Design
- [카카오 기술블로그 참고](https://fe-developers.kakaoent.com/2022/220505-how-page-part-use-atomic-design-system/)
- 계층형 구조를 몇가지 카테고리로 묶은 방법
- 계층 구조를 기반으로 컴포넌트들을 작성하는 것은 아토믹 디자인에서 중요한 기초 작업이다.
- 원자, 분자, 유기체, 템플릿, 페이지의 개념은 이 계층 구조를 형성하는 데 도움이 된다.
- 개념적인 것이고, 무조건 다 지킬 필요는 없지만 상황에 따라 아토믹 디자인 시스템의 장점 활용해보면 좋을 것 같다.

**원자(Atom)**
- 더이상 분해할 수 없는 기본 컴포넌트로, 버튼, 제목, 텍스트 입력 필드와 같은 가장 작은 구성 컴포넌트이다.

**분자(Molecule)**
- 여러 개의 atom을 결합하여 자신의 고유한 특성을 가진다.
- 중요한 점은 한 가지 일을 하는 것.
- molecule의 SRP는 재사용성과 UI에서의 일관성이 있다는 이점이 있다.

**유기체(Organism)**
- 예를 들어 header 라는 컨텍스트에 logo(atom), navigation(molecule), search form(molecule)을 포함할 수 있다.
- atom, molecule에 비해 좀 더 구체적으로 표현되고 컨텍스트를 가지기 때문에 상대적으로 재사용성이 낮아지는 특성을 가진다.

**템플릿(Template)**
- 실제 컴포넌트를 레이아웃에 배치하고 구조를 잡는 와이어 프레임
- 실제 콘텐츠가 없는 page 수준의 스켈레톤

**페이지(Page)**
- 유저가 보는 페이지

**장점**


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
### 1단계: UI를 구성 요소 계층 구조로 나누기

위에서 서술했다싶시피, JSON이 잘 구조화되어 있으면 자연스럽게 UI의 구성 요소 구조에 매핑되는 경우가 많기 때문에
UI와 데이터 모델은 종종 동일한 아키텍쳐를 띈다. 각 구성 요소가 데이터 모델의 한 부분과 일치하는 구성 요소로 UI를 분리하자.

계층 구조를 정렬해보는 예시
- `FilterableProductTable`
  - `SearchBar`
  - `ProductTable`
    - `ProductCategoryRow`
    - `ProductRow`
  
## 2단계: React에서 정적 버전 빌드하기
- 이 단계에서는 상태를 사용하지 말고, props로 처리해준다.
> 상태는 상호 작용, 즉 시간이 지남에 따라 변경되는 데이터에만 예약되어 있기 때문에, 정적 버전에서는 필요하지 않다.
- 계층 구조에서 아래에서 위로, 일반적으로 하향식으로 진행하는 것이 더 쉽지만, 대규모 프로젝트는 상향식으로 진행하는 것이 낫다.
- 리액트가 컴포넌트 인스턴스를 식별하는 방법으로 `key`가 있기 때문에, `key`는 유니크 해야한다.

```
 const categories =  products.reduce((acc, product) => (
  acc.includes(product.category) ? acc : [...acc, product.category]
), []);
```
reduce(누적된거, 현재아이템) : 
=> 지금 받은 product의 카테고리가, 이미 누적된 값 안에 포함되어있다면
추가안하고, 없다면 추가함

덩어리들을 충분히 잘게 나눌 수 있음.
여러개를 쪼개고, 쪼갠 것들이 어떤 데이터를 취하게 될 것인가를 고민하면서 만든다.

정규표현식에서 글로벌 설정:  / / g

**utils**
- 길게 코드를 쓰고 적절히 자를 수 있는 부분에서 함수로 추출한다.
- 하나씩 함수로 뺀 다음 파일을 만들어도 된다.
```
## 생각해 볼 것
- 카테고리가 이름과 product를 처음부터 갖게 했다면, 훨씬 단순해진다.
- `selectProducts`이런 함수도 필요 없을 수 있다.
이미 선택 된 값이 넘어오기 때문임.

