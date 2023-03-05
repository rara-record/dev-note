# Testing Library

테스트 코드 작성 후 다음 명령어를 입력하면 테스트가 진행된다.

```shell
npx jest
```

지속적으로 테스트 진행

```shell
npx jest --watchAll
```

더 자세하게 테스트를 보여줌

```shell
npx jest --verbose
```

둘다!

```shell
npx jest --watchAll --verbose
```

## 테스트 코드

```js
test('add', () => {
  expect(add(1, 2)).toBe(3);
});
```

### BDD 스타일의 테스트 코드

```js
describe('add', () => {
  it('returns sum of two numbers', () => {
    expect(add(1, 2)).toBe(3);
  });
});
```

context를 사용하면 더 자세하게 표현할 수 있다.

테스트코드를 작성할때는 최대한 세세하게 궁금증을 가지면서 최대한 많은 것들은 작성해가며 테스트를 하는 것이 좋다.

### 읽어볼 자료

- [jest](https://jestjs.io/)
- [React Testing Library 공식문서](https://testing-library.com/docs/react-testing-library/intro/)

F/E 테스트를 할때 react 컴포넌트 테스트만 하면 안된다.

### 추후 정리 내용

- TDD
- BDD
- E2E

### 정리

테스트 코드 작성 및 활용에 대해 강의를 들었으나 아직 정확하게 어떻게 어떤상황에서 작성해야 되는지 감이 오질 않아 추후에 자세히 알게 되면 다시 정리를 해야할 것 같다.
