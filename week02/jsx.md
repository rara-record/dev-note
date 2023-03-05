# JSX

XML-like stntax extension to ECMAScript
JSX는 JavaScript XML의 약자로 자바스크립트에서 XML을 추가한 확장형 문법
즉 jsx는 JavaScript 확장 문법
jsx는 xml처럼 작성된 부분을 React.creteElement를 쓰는 js 코드로 변환한다. 중괄호를 써서 js코드로 쓸 수 있고, 결국은 js 코드와 1:1로 매칭된다.

## React에서 JSX를 사용하는 목적

자바스크립트에서 HTML 문법을 사용할 수 있다는 점이고 주로 리액트나 다른 프론트엔드 프레임워크에서도 사용이 가능
일반적으로 JavaScript 코드 안에서 UI 관련 작업을 할 때 시각적으로 더 도움이 된다고 한다.

## Syntactic sugar

JSX는 컴포넌트와 해당 컴포넌트에 전달 될 props 및 하위 요소를 전달하기 쉽도록 도와주는 일종의 syntactic sugar의 역할을 수행할 수 있게 해준다. js와 다르게 동작하지 않지만 보다 알아보기 쉽다.

## React.createElement

리액트 element를 만든다.
jsx 없이 리액트를 사용하려면 사용해야 한다.

example #1

```jsx
<p>Hello, world!</p>
```

-> 다음으로 변환

```js
React.createElement('p', null, 'Hello, world');
```

example #2

```jsx
<Greeting name="world">hello</Greeting>
```

-> 다음으로 변환

```js
React.createElement(Greeting, { name: world }, 'hello');
```

example #3

```jsx
<div className="test">
  <p>Hello, world</p>
  <Button type="sumbit">Send</Button>
</div>
```

-> 다음으로 변환

```js
React.createElement(
  div,
  { className: world },
  React.createElement('p', null, 'Hello, world'),
  React.createElement(Button, { type: 'submit' }, 'Send')
);
```

## React Element

element는 React 앱의 가장 작은 단위
element는 화면에 보여줄 내용을 작성한다.

```js
const element = <h1>Hello, world</h1>;
```

브라우저 DOM 엘리먼트와 달리 React 엘리먼트는 일반 객체이며(plain object) 쉽게 생성할 수 있습니다.

## React StrictMode

StrictMode는 애플리케이션 내의 잠재적인 문제를 알아내기 위한 도구라고 한다.
개발환경에서만 동작하고 프로덕션 빌드에는 영향을 주지 않는다.

어디에서나 적용할 수 있다.

```js
import React from 'react';

function ExampleApplication() {
  return (
    <div>
      <Header />
      <React.StrictMode>
        <div>
          <ComponentOne />
          <ComponentTwo />
        </div>
      </React.StrictMode>
      <Footer />
    </div>
  );
}
```

다음과 같은 동작들을 검사하고 경고하는데 추후에 알아보자

- 안전하지 않은 생명주기를 사용하는 컴포넌트 발견
- 레거시 문자열 ref 사용에 대한 경고
- 권장되지 않는 findDOMNode 사용에 대한 경고
- 예상치 못한 부작용 검사
- 레거시 context API 검사
- Ensuring reusable state

## VDOM(Virtual DOM)이란?

virtual dom은 UI의 가상적인 표현을 메모리에 저장하고 reactDom과 같은 라이브러리에 의해 실제 Dom과 동기화 하는 프로그래밍 개념
이 과정을 재조정(Reconciliation)라고 한다.

실제 dom을 추상화한 자바스크립트 객체를 구성하여 사용하고
dom의 상태를 메모리에 저장하여, 상태의 변경 전 후를 비교해 변경된 부분만 다시 렌더시킨다.

특징

- 선언적api를 가능하게 한다.
- 유지보수하기 용이하다.
- 충분히 빠르다

### DOM이란?

Dom(Document object model)은 웹 페이지를 이루는 element들을 자바스크립트로 접근할 수 있게 브라우저가 트리구조로 만든 객체 모델

### DOM과 Virtual DOM의 차이

dom은 element가 변경되면 전체 문서가 갱신되지만 virtual dom은 해당 부분만 변경됨.

## Reconciliation(재조정) 과정은 무엇인가?

react에서 prop이나 state가 변경될때 직전 렌더링된 element와, 변경되고 만들어진 element를 비교하교 다르면 새로운 element로 실제 dom을 업데이트 하는 과정

이때 diff algorithm을 사용하는데 추후 다시 알아보자.

### 추후 정리 내용

- diff algorithm
- StrictMode의 검사 내용
