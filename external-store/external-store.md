# External Store

## 학습 키워드

- 관심사의 분리
- Layered Architecture
- Flux Architecture
- useReducer
- useCallback

## 관심사 분리 (Separation of Concerns)

관심사의 분리
정보를 잘 정의된 인터페이스가 있는 코드 부분 안에 캡슐화 시킴으로써 달성한다.

> 캡슐화: 정보숨기기의 한 수단

### 장점

관심사가 잘 분리될 때 모듈 재사용을 위한 더 높은 정도의 자유가 있다.
모듈이 인터페이스 뒤에서 관심사의 세세한 부분은 숨기기 때문에, 다른 부분의 세세한 사항을 모르더라도
하나의 관심사의 코드 부분을 개선하거나 수정할 수 있게 된다.
하여 중간의 기능 손실 없이 시스템을 업그레이드 하는데 자유도를 높여준다.

### 단점

관심사 분리라는 것이 추상화의 일종인데, 추상화는 인터페이스의 추가는 필수이고 실행에 쓰이는 더 순수한 코드가 있는 것이 일반적이라 잘 분리된 관심사의 여러 장점에도 불구하고 관련 실행에 따른 불이익이 있기도 하다.

## 계층화 아키텍쳐 Layered Architecture

- Layered architecture = 다층 구조 = 목적에 따라 모듈을 나눈 것
- 사용자에게 가까운 것과 사용자에게서 먼 것으로 구분
- 계층화 아키텍쳐는 OOP보다 조금 더 좁은 관점으로 볼 수 있다.

### ReactJS layer architecture

> [React Case](https://dev.to/azu/almin--reactvue-can-optimizing-performance-visually-541)

> [다층 구조를 사용하여 React 앱 최적화](https://imagineu.tistory.com/82)

> [클린 아키텍쳐](https://dev.to/daslaf/clean-architecture-for-react-apps-3g3m)

1. 렌더링 하는 영역에서 로직을 빼고
2. API 콜하는 곳에서 로직을 빼고
   애플리케이션 코드가 지켜야 할 영역과 외부에 영향이 있는 곳을 분리하여 격리화 한다.

> 외부에 있는 것은 제어할 수 없으니, UI 담당 부분(UI Layer)이나, UseCase Layer처럼 테스트코드로 지켜줄 수 있는 곳은 완전히 지켜준다.

### 당장 무엇을 적용해볼 수 있을까.

1. 최소한 http 요청하는 공통 모듈, API 모듈, axios를 추상화한 클래스, 서비스 레이어, 데이터 요청 응답레이어만이라도 나눠본다.
2. 의존성이 없는 레이어로 나누라는 것을, 각 레이어에 존재하는 변수를 다른 레이어에서 사용하지 않는다고 이해해보자.

## Flux Architecture

1. action : 이벤트/메세지같은 객체
2. deispatcher : 여러 store로 action을 전달. 메세지 브로커와 유사하다
3. store (여러개) : 받은 action에 따라 상태를 변경
4. view: store를 구독하고 있는 view, (구독 : 변화가 있는지 없는지 감지 후 반영)

### 리덕스

store 1개
store에서는 action을 받는다.
리듀서를 통해 state 상태를 새거로 바꿔줌.
const state = {
name: 'tester'
}

```javascript
const nextState = { ...state, name: "New Name" };
```

## External Store란?

리듀서가 기본형
setState가 내부적으로 useReducer를 쓴다

forceupdate
