# React Hook


## 학습 키워드

- React Hook 이란
- Hooks
    - useState
    - useEffect
    - useContext
    - useRef
    - useLayoutEffect
- React StrictMode 란

## React의 Hook이란?
React 16.8에서 추가된 개념으로, 
class를 작성하지 않고도 state와 같은 React 기능들을 사용할 수 있다.
ref.
- [Hook 소개](https://ko.reactjs.org/docs/hooks-intro.html)
- [Hook의 개요](https://ko.reactjs.org/docs/hooks-overview.html)
- [Hooks API Reference](https://ko.reactjs.org/docs/hooks-reference.html)

기존 방식의 문제점:
1. Wrapper Hell (HoC)
  - 컴포넌트의 재구성을 강요하며, 코드의 추적을 어렵게 한다. 따라서 다른 추상화에 대한 레이어로 둘러싸인 래퍼 지옥 즉, wrapper hell을 볼 가능성이 있다. 
2. Huge Components
- Confusing Classes

현재 방식: 
- 기존에는 상태를 가진 컴포넌트는 Class로 만들고, 
props만 사용하는 재사용이 용이한 작은 컴포넌트는 function 컴포넌트로 작성했지만 
현재는 function만 쓰기 때문에, 상태 관리 유무를 바로 알기는 어렵다, 
> 즉, 신경을 많이 쓰지 않아도 된다.
- 섣부른 최적화(메모이제이션)을 피할 것
> useCallback과 useMemo 사용을 고려해야하는 사례들 링크:  [최적화가 필요하지 않은 이유][관련 링크](https://kentcdodds.com/blog/usememo-and-usecallback)
- 복잡한 요소는 전부 Hook으로 격리 및 재사용 가능하다.

## useEffect
컴포넌트
리액트
브라우저

첫번째 랜더링 숫자0


1. 렌더링 이후 해야 할 일, `즉 React의 외부와 관련된 일`을 정해줄 수 있다. 
### useEffect는 이벤트 핸들러와 어떻게 다른 것인가?
렌더링 코드: props와 상태를 가져와 변환하고, 화면에 표시하려는 JSX를 반환한다.
렌더링 코드는 순수해야 하며, 계사된 결과를 실행할 뿐 다른 작업은 수행하지 않는다.

이벤트 핸들러는 컴포넌트 내부에 있는 중첩된 함수로, 
입력 필드를 업데이트 하거나, HTTP POST 요청을 제출하여, 사용자를 다른 화면으로 이동할 수 있다.
이벤트 핸들러에는 부수효과 (프로그램 상태변경)가 포함되어 있으며
특정 사용자 작업 (예: 버튼 클릭 또는 입력)으로 인해 발생한다.

화면에 표시 될 때마다 채팅 서버에 연결해야 하는 컴포넌트를 생각해 보면,
서버에 연결하는 것은 순수한 계산이 아니므로 렌더링 중에 발생할 수 벗다.



2. 기본적으로 렌더링 때마다 실행되므로, 의존성 배열을 통해 언제 이펙트를 실행할지 지정할 수 있다.
3. 함수를 리턴함으로써 종료 처리를 할 수 있다.

ref.
- [Synchronizing with Effects](https://beta.reactjs.org/learn/synchronizing-with-effects)
- [useEffect가 필요하지 않을 수 있다](https://beta.reactjs.org/learn/you-might-not-need-an-effect)
- [useEffect 완벽 가이드](https://overreacted.io/ko/a-complete-guide-to-useeffect/)
- [Youtube: All useEffect Mistakes Every Junior React Developer Makes](https://www.youtube.com/watch?v=QQYeipc_cik&t=980s&ab_channel=LamaDev)

### useEffect 잘 사용하는법
1. 어떤한 함수를 이펙트 안에서만 쓴다면, 그 함수를 직접 이펙트 안으로 옮길 것
```typescript jsx
function SearchResults() {
  const [query, setQuery] = useState('react');

  // 이 함수가 길다고 상상해 봅시다
  function getFetchUrl() {
    return 'https://hn.algolia.com/api/v1/search?query=' + query;
  }

  // 이 함수가 길다고 상상해 봅시다
  async function fetchData() {
    const result = await axios(getFetchUrl());
    setData(result.data);
  }

  useEffect(() => {
    fetchData();
  }, []);

  // ...
}
```
useEffect는 클로저라서, effect가 맨 처음에 잡히면 
바깥에 있던 변수들을 캡쳐하고 바인딩 해서, 안에서 쓰기 때문에 문제가 생길 수 있다.

만약 이 코드에서, props와 state에 대한 의존성을 추가하는 것을 깜빡 했다면,
effect는 변화에 대한 동기화를 실패할 것이다.

가장 쉬운 해결책은 함수를 이펙트 안에서만 쓰고, 나중에 수정 사항이 있더라도
effect 안에 있는 함수들만 신경 쓰면서, 의존성 배열을 관리할 것.

## React StrictMode






