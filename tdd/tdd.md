# TDD

## 학습 키워드
- TDD란
- Jest
- Describe - Context - It 패턴
- 단위테스트란
- TDD에서 중복을 제거한다는 게 어떤 의미인가

테스트 주도 개발
테스트 코드를 먼저 작성한다.
구현보다 인터페이스와 스펙을 먼저 정의하면서 개발을 진행하는 방식.
막연하게 하는 것이 아니라, 기대값을 생각하면서.

테스트 코드를 먼저 쓰면 무조건 TDD라는 건 아님

1. Red
2. Green
3. Refactor: 제대로 작동하는 클린코드. 동작은 바뀌지 않고
설계는 계속해서 바뀌는 것.

의도를 드러내면 저절로 중복이 드러나는 경우가 있어서,
모아서 처리할 수 있다.

훈련방법
- 작은 단계를 찾고, 중복을 일부러 만들어내서, 패턴을 찾아내서 중복을 제거하고 정리한다

## Jest
자바스크립트 환경에서는 jest를 다 사용할 수 있다.

### 특징
1. 복잡한 설정이 필요 없다
2. snapshots test를 지원한다
- 단위테스트와는 별개이다.
- 어떠한 object를 캡쳐할 수 있는, 즉  

Jest에서 typescript 사용하도록 파일 추가
```
jset.config.js
```
```javascript
module.exports = {
	testEnvironment: 'jsdom',
	setupFilesAfterEnv: [
		'@testing-library/jest-dom/extend-expect',
	],
	transform: {
		'^.+\\.(t|j)sx?$': ['@swc/jest', {
			jsc: {
				parser: {
					syntax: 'typescript',	
					jsx: true,
					decorators: true,
				},
				transform: {	
					react: {
						runtime: 'automatic',
					},
				},
			},
		}],
	},
};
```

Jest 실행
```
npx jest

npx jest --watchAll
```

## Given-When-Then
테스크 코드 템플릿.
> 700W 전자렌지를 준비해서 3분만 돌리면 완성!

코드로 보기
```
# Given ~ 하고, ~ 할때
stove = Stove.new(700.watts)

# When ~ 하면
food = stove.cook(3.minutes)

# Then ~ 가 된다, ~를 한다.
assert food.complete?
```

## 유닛 테스트
단위 테스트.
> [jest 공식문서](https://mulder21c.github.io/jest/docs/en/next/getting-started.html)
> 

> 테스트 코드를 작성하는 방법은 크게 두가지 이다.
1. test 함수로 개별 테스트를 나열한다.

 개별 테스트 코드 작성 해보기
```jset
// 함수 정의
const add = (x: number, y: number): number => {
  return 0;
}


test('add 함수는 두 훗자를 더한다.' () => {
  expect(add(1, 2)).toBe(3);
});


expect() // 기대 함수
toBe() // 결과값
```

2. BDD 스타일로 대상과 행위를 명확히 드러낸다.
```jset
describe('add', () => { 
  it('add 함수는 두 숫자를 더해서 리턴한다', () => {
    expect(add(1, 2))
      .toBe(3);
  });
});

add // 주어
it // 설명
add 함수는 ~ it ~한다.
```
재귀로 하는 것들은, reduce를 쓰면 거의 똑같이 됨.

## 리액트 테스트 라이브러리
리액트 컴포넌트를 사용자 입장에 가깝게 테스트할 수 있는 도구이다.

