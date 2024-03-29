# React Testing Library

### 학습 키워드

- React Testing Library
- given - when - then 패턴
- Mocking
- Test fixture

## 1. React Testing Library

Jest의 대체품이 아니며, React 구성 요소(component 등)을 테스트하기 위해 만들어진 제작된 JavaScript 테스트 유틸리티이다.
하여, 리액트 컴포넌트를 테스트하려면 Jest + React Testing Library가 필요하다.
react-testing-library는 리액트 컴포넌트 testing을 위한 가상 돔(Virtual DOM)을 제공해준다.

```text
--npx tsc --noEmit : 원래 컴파일한 결과가 js 파일로 생기는데 안생기게 하는 명령어
```

## 1-1. Mocking

### mocking이란 무엇인가?

> [모킹 정리 블로그](https://inpa.tistory.com/entry/JEST-%F0%9F%93%9A-%EB%AA%A8%ED%82%B9-mocking-jestfn-jestspyOn)

Mock Service Worker의 약자. 서비스 워커의 기능을 유용히 활용한 것이다.
mocking은 단위 테스트를 작성 할 때 해당 코드가 의존하는 부분을 가짜로 대체하는 기법을 말한다.

일반적으로 테스트하려는 코드가 의존하는 부분을 직접 생성하기가
힘든 경우 mocking이 많이 사용된다.

> jest.fn: 가짜 함수를 생성할 수 있도록 `jest.fn()`함수를 제공한다.

> jest.spyOn()도 있다.

### Jest에서 Mock을 정리하는 법

다음 테스트 케이스를 실행하기 전에 현재 테스트케이스에서 사용했던 mock을 정리해 주는 것이 좋다.

### 왜?

- 다음 테스트 케이스에 영향을 줄 수 있기 때문이다.

```javascript
const consoleLog = console.log;
test("spyOn으로 console.log를 mocking하면, console.log는 다른 함수가 된다.", () => {
  jest.spyOn(console, "log");
  const consoleLogAfterMocking = console.log;

  // 결과는 Success.
  // console.log는 spyOn으로 mocking한 이후 다른 함수가 된다.
  expect(consoleLog).not.toBe(consoleLogAfterMocking);
});
```

모든 테스트 케이스가 독립적이게 하기 위해서 mock을 정리한다.

### 어떻게?

Jest에서 테스트 코드에서 수동으로 mock을 정리하는 방법
[jest mockClear 정리 블로그](https://haeguri.github.io/2020/12/21/clean-up-jest-mock/)

- `mockFn.mockClear`
- `mockFn.mockReset`
- `mockFn.mockRestore`

### 이벤트 관련 API

[event api](https://github.com/testing-library/dom-testing-library/blob/main/src/event-map.js)

> fireEvent: 컴포넌트 내부에서 이벤트 핸들러를 구현하지 않고, 외부에서 받아오는 경우

```javascript
// 이벤트 쓰는법
fireEvent.change(screen.getByLabelText(label), {
  target: { value: "New name" },
});

// setText 여부가 call 되었는지
expect(setText).toBeCalledWith("New Name");

// 한번에 하나의 함수만 테스트해야 하고,
// 만약 중복으로 사용하는 코드는
// mockCleaer()
beforeEach(() => {
  setText.mockClear();
  // 또는 jest.clearAllMocks();
});
```

> 반복 되는 코드는 Extract Function한다.

> fireEvent 등을 통해 인터렉션만 검증할 것

## 1-2. 컴포넌트 테스트 코드를 작성하는 법

먼저 TextField 컴포넌트 테스트 코드를 작성해보자.

```typescript jsx
import { render, screen } from "@testing-library/react";

import TextField from "./TextField";

test("TextField", () => {
  const text = "Tester";
  const setText = () => {
    //
  };
  render(
    <TextField
      label='Name'
      placeholder='Input your name'
      // filterText -> text로 수정
      text={text}
      setText={setText}
    />
  );

  screen.getByLabelText("Name");
});
```

### Given-When-Then 패턴

이 코드를 BDD 스타일의 테스트 코드로 작성하려면, Given-When-Then 패턴을 사용한다.

- Given : 준비
- When : 실행
- Then : 결과
  테스트 코드 템플릿.
  > 700W 전자렌지를 준비해서 3분만 돌리면 완성!

코드로 보기

```
# Given : 준비, ( ~ 하고, ~ 할때 )
stove = Stove.new(700.watts)

# When :  실행,  (~ 하면 )
food = stove.cook(3.minutes)

# Then ~ : 결과값 확인,  (가 된다, ~를 한다.)
assert food.complete?
```

### given-when-then 패턴으로 구현하기

```typescript jsx
import { fireEvent, render, screen } from "@testing-library/react";

import TextField from "./TextField";

const context = describe;

// TextField가 테스트 대상
describe("TextField", () => {
  //given : 상태 변수등을 "준비" 한다.
  const label = "Name";
  const text = "Tester";

  const setText = jest.fn();

  beforeEach(() => {
    // setText.mockClear();
    jest.clearAllMocks();
  });

  // 반복되는 부분은 추출하여 분리한다.
  function renderTextField() {
    render(
      <TextField
        label={label}
        placeholder='input your name'
        text={text}
        setText={setText}
      />
    );
  }

  // 이벤트 함수 작성
  function inputText(value: string) {
    fireEvent.change(screen.getByLabelText(label), {
      target: { value },
    });
  }

  // 1. 렌더링 여부 테스트
  it("renders an input control", () => {
    //when : 텍스트 필드 함수를 렌더링 했을 "때"
    renderTextField();

    //then : 화면이 잘 뜨는 지 "결과"를 나타낸다.
    screen.getByLabelText("Name");
  });

  // 2. 이벤트가 동작 여부 성공 테스트
  //when : 사용자가 검색창에 이름을 입력하면
  context("when user enters name", () => {
    beforeEach(() => {
      renderTextField();
    });

    //then : setText 함수가 호출된다.
    it('calls "setText" handler', () => {
      inputText("New Name");

      expect(setText).toBeCalledWith("New Name");
    });
  });
});
```

## Test fixture

비지니스 로직같이 외부와 의존성이 큰 경우도
해당 부분만 가짜로 만들 수 있는데,
이 부분을 하나씩 구현하다보면 에로사항이 있을 수 있다.

```javascript
// App.test.ts

jest.mock("./hooks/useFetchProducts", () => () => [
  {
    category: "Fruits",
    price: "$1",
    stocked: true,
    name: "Apple",
  },
]);
```

백엔드와 소통할 떄마다, 매번 해당 hook 등을 불러와서, 써줘야 하기 때문에
해당 부분은 fixtures 이름으로 관리해준다.

```javascript
// fixtures/products.ts

const products = [
  {
    category: 'Fruits', price: '$1', stocked: true, name: 'Apple',
  }]

export default products

// fixtures/index.ts
import products from 'products'

export default {
  products
}
```

다시 App.test.ts로 돌아와서, fixture에 product를 불러온 후

```javascript
// App.test.ts

import fixture from "../fixture";

jest.mock("./hooks/useFetchProducts", () => () => fixture.products);
```

이렇게 수정해서 사용하여도 된다.

> 또다른 방법은, hooks 하위 폴더에 `__mock__` 폴더를 만들어서,

```javascript
jest.mock("./hooks/useFetchProducts", () => () => fixture.products);
```

해당 부분을 export하여, App.test.ts로 불러올 수 있다.
