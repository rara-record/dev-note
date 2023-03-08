## 학습 키워드

- TSyringe
- 의존성 주입(Dependency Injection)
- reflect-metadata
- sington (싱글톤)

> [TSyringe](https://github.com/microsoft/tsyringe)

> [reflect-metadata](https://github.com/rbuckton/reflect-metadata)

> [The problem with passing props](https://beta.reactjs.org/learn/passing-data-deeply-with-context#the-problem-with-passing-props)

## 1. TSyringe 
Typescript용 가벼운 DI도구(IoC Container),
External Store를 관리하는데 활용할 수 있다.
> DI란? Dependency Injection의 약자로 의존성 주입을 말한다.

React에서 잘 쓰면 “전역”처럼 쓸 수 있다.
“Prop Drilling” 문제를 우아하게 해결할 수 있는 방법 중 하나

React로 한정하면 Context도 사용할 수 있지만,
해당 Context가 바뀌면 하위요소가 모두 리렌더링 되기 때문에 비효율적인 측면이 있다.
(forceUpdate에 가깝기 때문) 

싱글톤을 사용하기 떄문에 전역에서 하나.어디서든지 하나.
자동으로 ioc 컨테이너가 객체 생성을 알아서 해주고 
만약 스토어가 다른 것에 의존하면 알아서 조립을 다 해준다 (팩토리 기능까지)

```text
npm i tsyringe reflect-metadata
```
src/main.tsx 파일과 src/setupTests.ts 파일에서 가져오기.
> `reflect-metadata`를 써줘야하는 위치 : 모든 프로그램이 시작하는 곳
```typescript jsx
// src.main.tsx 
// src/setupTests.ts
import 'reflect-metadata';
```

## 1-1. 사용 방법
sington class를 만든다.
```typescript
import { singleton } from 'tsyringe';

@singleton()
class CounterStore {
	// …(중략)...
}
```

```typescript js
import { container } from 'tsyringe';
const counterStore = container.resolve(CounterStore);
```

@ : 데코레이터
데코레이터를 써주려면 tsconfig에서, 주석 풀어주기
```json
- experimentalDecorators: true
- emitDecoratorMetadata: true
```
## 1-2. 싱글톤
 
