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
2. 관심사 분리라는 것이 추상화의 일종인데, 추상화는 인터페이스의 추가는 필수이고 실행에 쓰이는 더 순수한 코드가 있는 것이 일반적이라
잘 분리된 관심사의 여러 장점에도 불구하고 관련 실행에 따른 불이익이 있기도 하다.

   
## 계층화 아키텍쳐 Layered Architecture

Layered Architecture에선 사용자에게 가까운 것과 사용자에게서 먼 것으로 구분


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
const nextState = { ...state, name: 'New Name'}
```

## External Store

리듀서가 기본형
setState가 내부적으로 useReducer를 쓴다

forceupdate




