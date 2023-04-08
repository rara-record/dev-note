# TDD

## 학습 키워드

- TDD란
- Jest
- Describe - Context - It 패턴
- 단위테스트란
- TDD에서 중복을 제거한다는 게 어떤 의미인가

## 1. TDD(테스트 주도 개발)

[TDD-방법론-테스트-주도-개발](https://inpa.tistory.com/entry/QA-%F0%9F%93%9A-TDD-%EB%B0%A9%EB%B2%95%EB%A1%A0-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%A3%BC%EB%8F%84-%EA%B0%9C%EB%B0%9C)

테스트는 우리의 코드가 기대하는 대로 동작하는지 확인하는 것인다.
그렇다면, 테스트는 언제 작성해야 하는가?
테스트 주도 개발은, 테스트 코드를 먼저 작성하는 방법론으로
테스트 코드를 먼저 작성하는 개발 방법론은 테스트 주도 개발(Test-Driven Development, TDD)로 많이 불린다.
좋은 테스트의 특징 중 Timely, 테스트는 적시에 즉, 테스트 하려는 실제 코드를 구현하기 직전에 구현해야 한다는 말이 있다.
왜 그럴까?
TDD의 궁극적인 목표는,

1. 원하는 대로 작동하는
2. 깔끔한 코드를 작성하는 것이다.

현실적인 측면엔서, 프로덕션 코드를 먼저 작성 한 후 진행하려면

1. 예상한대로 동작하지 않을 경우 다시 설계하고 테스트해야 하는 번거로움
2. .. 매우 귀찮아진다는 것.

테스트 코드를 작성하는 방법을 큰 줄기에서 보면,

1. 실패 테스트 부터 작성한다
2. 순차적으로 실패하는 테스트 작성 후, 오직 실패 할 경우에만 새로운 코드를 작성한다.
3. 중복된 코드는 제거한다.

## 1-1. 좋은 테스트 원칙

테스트 코드를 먼저 작성하여, 구현보다 인터페이스와 스펙을 먼저 정의하면서 개발을 진행하는 방식을 말한다.

1. Red
2. Green
3. Refactor: 제대로 작동하는 클린코드. 동작은 바뀌지 않고
   설계는 계속해서 바뀌는 것.

의도를 드러내면 저절로 중복이 드러나는 경우가 있어서,
모아서 처리할 수 있다.

### 훈련방법

- 구현보다 인터페이스와 스펙을 먼저 정의하면서 개발을 진행하는 방식.
- 막연하게 하는 것이 아니라, 기대값과 유지보수를 생각하면서 작성한다.
- 지나치게 내부 구현 사항을 테스트 하지 말것.
- 재사용성을 높이기 (테스트 유틸리티)
- 배포용 코드와 철저히 분리한다.
- 테스트 코드는 어느정도 문서화라는 것을 잊지 말기

### 1-2. 무엇을 테스트 해야 할지 모를 때의 법칙

테스트의 범위

**BICEP**

1. 모든 코너 케이스에 대해 테스트 하기 (Boundary conditions)
   > 잘못된 포맷의 인풋, 특수문자, null, 잘못 된 이메일, 작은 숫자, 중복, 순서가 맞지 않을 때 등등.
2. 역관계를 적용해서 결과값을 확인하기 (Inverse relationship)
   > 일관성을 유지. (덧셈은 뺄셈, 추가는 제거)

> ex) 5+5 테스트 후, 10에서 5를 뺴보기. 학생을 추가해서 배열에 추가하면
> 반대로 그 학생을 제거한 후 배열을 확인하기. (Cross-check)

3. 다른 수단을 이용해서 결과값이 맞는지 확인
   > 여러가지를 이용해서 결과값을 확인한다.

> ex) 추가 된 과일 == 전체과일 - 예전의 과일 갯수
>
> A 알고리즘 == B 알고리즘

4. 예상할 수 있는 모든 에러 케이스에 대해 통과하는지 (Error conditions)

   > 네트워크 에러, 메모리 부족, ...

5. 성능 확인은 테스트를 통해 정확한 수치로 확인한다. (Performance)
   > 성능 개선의 척도와 확인도 데이터를 통해 확인한다.

### 1-3. 테스트의 조건

**CORRECT**

1. Conformance
   > 특정 포맷을 준수: 전화 번호, 이메일, 아이디..
2. Ordering
   > 순서 조건 확인하기
3. Range
   > 숫자의 범위 : 제한된 범위보다 작거나 큰 경우
4. Reference
   > 외부 의존성 유무, 특정한 조건의 유무, 가정하는 조건에 반할 떄

> ex) B함수 호출 시 A함수를 먼저 실행해야 하는 경우, A함수가 먼저 실행되지 않을 경우에는 어떻게 할 것인지. 5. Existence
> 값이 존재 하지 않을 때
> null, undefined, '', 0 6. Cardinality
> 0-1-N의 법칙
>
> 하나도 없을 때, 하나만 있을 때, 여러개 있을때 7. Time
> 상대, 절대 동시의 일들
> 순서가 맞지 않은 경우, 소비한 시간, 지역 시간

### 1-4. TDD Cycle

1. Red : 실패하는 작은 단위 테스트를 작성한다. 인터페이스와 스펙에 집중
2. Green :
   - 빨리 통과하기 위해 프로덕션 코드를 작성한다 (정답이 아닌 가짜 구현도 괜찮다)
   - 그 후 실패 없는 테스트 코드를 작성한다.
   - 반복하며, 실패/성공의 모든 테스트 케이스를 작성해본다.
3. **Refactor** :
   - 개발 된 코드들에 대해 모든 중복을 제거하며 리팩토링 한다.
   - 코드를 깔끔하게 만들어 나간다.

## 2. Jest

Jest는 페이스북에서 만들어서 React와 더불어
많은 자바스크립트 개발자들로 부터 좋은 반응을 얻고 있는 테스팅 라이브러리다.
출시 초기에는 프론트앤드에서 주로 쓰였지만 최근에는 백앤드에서도 기존의 자바스크립트 테스팅 라이브러리를 대체하고 있다.

[jest 공식문서](https://mulder21c.github.io/jest/docs/en/next/getting-started.html)

> Jest 이전에는 자바스크립트 코드를 테스트하라면 여러가지 테스팅 라이브러리를 조합해서 사용하곤 했었다.
> 예를 들어, Mocha나 Jasmin을 Test Runner로 사용하고, Chai나 Expect와 같은 Test Mathcher를 사용했으며, 또한 Sinon과 Testdouble 같은 Test Mock 라이브러리도 필요했었다.
> 이 라이브러리들은 굉장히 유사하지만 살짝씩 다른 API를 가지고 있었기 때문에, 여러 프로젝트에 걸쳐서 일하는 자바스크립트 개발자들에게 혼란을 주기도 했었다.

하지만 Jest는 라이브러리 하나만 설치하면, Test Runner와 Test Mathcher 그리고 Test Mock 프레임워크까지 제공해주기 때문에 현재 대세라고 말할 수 있다.

## 2-1. JEST 설치

```text
> npm i -D jest
```

jset.config.js 설정하기

```javascript
module.exports = {
  testEnvironment: "jsdom",
  setupFilesAfterEnv: ["@testing-library/jest-dom/extend-expect"],
  transform: {
    "^.+\\.(t|j)sx?$": [
      "@swc/jest",
      {
        jsc: {
          parser: {
            syntax: "typescript",
            jsx: true,
            decorators: true,
          },
          transform: {
            react: {
              runtime: "automatic",
            },
          },
        },
      },
    ],
  },
};
```

설치를 완료한 뒤에, package.json 파일을 열고 test 스크립트를 jest로 추가해 주자
npm run test 대신 바로 npm test 로 스크립트 실행이 가능하며,
계속 실행해야 하는 번거로움때문에 따로 스크립트 부분에 설정해준다.

터미널에서 바로 실행하기

```
npx jest

npx jest --watchAll
```

스크립트 파일에 추가해서 사용하기

```text
"watch:test": "jest --watchAll",
```

## 2-2. Jest 기본 문법

[Jest Docs](https://jestjs.io/docs/expect#reference)
먼저 test 파일을 만든다. test파일은 테스트할함수파일명.test.js 로 해준다.

```javascript
describe("계산 테스트", () => {
  const a = 1,
    b = 2;

  test("a + b는 3이다.", () => {
    expect(a + b).toEqual(3);
  });
});

/*
describe('그룹 테스트 설명 문자열', () => {
   const a = 1, b = 2; // 테스트에 사용할 일회용 가짜 변수 선언

   test('개별 테스트 설명 문자열', () => {
      expect(검증대상).toXxx(기대결과);
   });
});
*/
```

1. describe()

- describe 는 테스트 그룹을 묶어주는 역할을 하고, 그안의 콜백함수 내에 테스트에 쓰일 가자 변수,객체들을 선언하여 일회용으로 사용 할 수 있다.

2. test()

- 개별 테스트 케이스를 의미하며, it()과 같다.

3. expect()

- 인자로는 테스트할 대상을 넣고, 기대 값 등을 뒤에 메서드 체이닝으로 넣어준다.

4. toBe()

- toXxx 부분에서 사용되는 함수를 흔히 Test Mathcher라고 하며, 기대값을 나타내는데,
- Jest는 toBe() 대신에 toEqual() 함수를 사용하라고 가이드해주고 있다.
- 실제로 테스트 코드를 작성할 때는 객체를 검증해야할 일이 많기 때문에 toEqual() 함수를 가장 많이 접할 수 있다.

5. beforeEach()

- 테스트 파일의 테스트 코드가 돌기 전에 수행할 로직을 넣어준다. 테스트 케이스마다 반복되는 로직이 있는 경우 사용하면 유용하다.

6. afterEach()

- beforeEach와 반대이다. 테스트 함수가 끝나면 호출된다.

### 유용한 Matcher 정리

[유용한 Matcher 함수 종류 모음](https://inpa.tistory.com/entry/JEST-%F0%9F%93%9A-jest-%EA%B8%B0%EB%B3%B8-%EB%AC%B8%EB%B2%95-%EC%A0%95%EB%A6%AC)

## 2-3. 테스트 코드를 작성하는 방법

방법으로는 크게 두가지가 있는데, 개별 테스트로 '나열'하는 방식과,
주체-행위, 그룹안에 개별 개별 개별이 존재하게 조직화 한다.

1. test 함수로 개별 테스트를 나열한다.

```jset
// 함수 정의
const add = (x: number, y: number): number => {
  return 0;
}


test('add 함수는 두 훗자를 더한다.' () => {
  expect(add(1, 2)).toBe(3);
});
```

2. BDD 스타일로 대상과 행위를 명확히 드러낸다.

```jset
describe('add', () => {
  it('add 함수는 두 숫자를 더해서 리턴한다', () => {
    expect(add(1, 2))
      .toBe(3);
  });
});

```

### BDD 스타일의 Describe - Context - It 패턴

1. Describe(설명 할 테스트 함수명 등을 넣는다)
2. Context(테스트 대상의 상황을 설명한다 : when/with 자주 사용 )
3. it(테스트 대상의 결과값을 설명한다 : 리턴값)

```javascript
function add(...numbers: number[]): number {
  if (!numbers.length === 0) {
    return 0;
  }
  // if (numbers.length === 1) {
  //   return numbers[0];
  // }
  // if (numbers.length === 2) {
  //   return numbers[0] + number[i];
  // }
  // if (numbers.length === 3) {
  //   return add(numbers[0], numbers[1]) + numbers[2];
  // }

  return numbers.reduce((acc, cur) => acc + cur);
}

const context = describe;

test("add", () => {
  expect(add(1, 2)).toBe(3);
});

// 테스트 대상의 함수명을 입력해준다.
describe("add", () => {
  // 테스트 상황을 설명한다.
  context("with no argument", () => {
    // 테스트 대상의 예상 결과값을 설명한다.
    it("return zero", () => {
      // 위에 써놓은 그대로 코드를 써준다.
      expect(add()).toBe(0);
    });
  });

  context("with only one argument", () => {
    it("returns the same number", () => {
      expect(add(2)).toBe(2);
    });
  });

  context("with two arguments", () => {
    it("return sum of two numbers", () => {
      expect(add(1, 2)).toBe(3);
    });
  });

  context("with three arguments", () => {
    it("returns sum of three numbers", () => {
      expect(add(1, 2, 3)).toBe(6);
    });
  });
});
```

그리고 npm test를 실행하면 프로젝트 내에 모든 테스트 파일을 찾아서 테스트를 실행해준다.
Jest는 기본적으로 test.js로 끝나거나, **test** 디렉터리 안에 있는 파일들은 모두 테스트 파일로 인식한다.
만약 특정 테스트 파일만 실행하고 싶은 경우에는 npm test <파일명 이나 경로>를 입력하면 된다.

## 3. 단위 테스트

단위 테스트(Unit Test)는 하나의 모듈을 기준으로 독립적으로 진행되는 가장 작은 단위의 테스트이다. 여기서 모듈은 애플리케이션에서 작동하는 하나의 기능 또는 메소드로 이해할 수 있다.
예를 들어 웹 애플리케이션에서 로그인 메소드에 대한 독립적인 테스트가 1개의 단위테스트가 될 수 있다.
즉, 단위 테스트는 애플리케이션을 구성하는 하나의 기능이 올바르게 동작하는지를 독립적으로 테스트하는 것으로, "어떤 기능이 실행되면 어떤 결과가 나온다" 정도로 테스트를 진행한다.
