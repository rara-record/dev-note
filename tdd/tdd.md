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
- 중복을 일부러 만들어내서, 패턴을 찾아내서 정리한다

## Jest
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

```typescript
// 함수 정의
const add = (x: number, y: number): number => { 
  return 0;
}

test.co

```

