# External Store

## 학습 키워드

- 관심사의 분리
- Layered Architecture
- Flux Architecture
- useReducer
- useCallback
- internal state / external state
- useSyncExternalStore

## 1. 관심사 분리 (Separation of Concerns)

관심사의 분리 <br />
정보를 잘 정의된 인터페이스가 있는 코드 부분 안에 캡슐화 시킴으로써 달성한다.
> 캡슐화: 정보숨기기의 한 수단
1. 코드는 단위별로 하나의 관심사만 갖도록 하고, 그 관심사에 대해서만 충실히 동작하도록 만들어야 한다.
2. 관심사의 분리가 적절히 구현된 코드에서는
   - 낮은 결합도, 각각의 코드가 서로 얽혀짔지 않고, 독립적으로 잘 분리되어 있다.
   - 높은 응집고, 유사한 내용끼리 비슷한 위치에 잘 모여있다.

## 2. View와 Logic의 분리
## 2-1. 리액트에서 관심사 분리
예전에는 Presentational - Container 패턴을 많이 사용했다.

### Presentational Component : UI Only 컴포넌트 (View)


```typescript jsx
const UserList = ({ users }) => {
    return (
        <ul>
            {users.map(user => {
                return <li key={user.id}>{user.name}</li>
            })}
        </ul>
    )
}
```
- state, effect 등 로직 없음 → 화면을 렌더링하는 데 필요한 코드만 존재 (갖더라도 UI에 대한 상태만)
- 함수 컴포넌트로 구현

### Container Component : Logic Only 컴포넌트 (Logic)
```typescript jsx
class UserListContainer extends React.Component {
	state = {
		users: []
	}

	componentDidMount() {
		fetchUsers("/users")
			.then(res => res.json())
			.then(res => this.setState({ users: res.users }))
	}

	render() {
		return <UserList users={this.state.users}>
	}
}
```
- 데이터 호출, state, 필터링 등 복잡한 로직을 처리하는 코드만 존재 (UI 관련 코드를 가져서는 안됨)
- 클래스형 컴포넌트로 구현

이 패턴이 제안될 당시에는 Hooks가 존재하지 않았다. 그래서 함수 컴포넌트는 State와 Effect를 처리할 수 없었고, UI를 그리는 데만 제한적으로 사용되었다. 반면 클래스형 컴포넌트는 State와 Life Cycle API(Effect에 대응됨)를 사용해 React의 모든 기능을 구현할 수 있었다.
=> 이렇게 View와 Logic을 분리함으로써 컴포넌트는 더 작은 영역에 대해 더 확실한 책임을 지는 여러 개의 컴포넌트로 분할된다.

View에 집중하는 Presentational Component는 단순히 주입 받은 정보를 렌더링할 뿐(ex. (state, props) ⇒ UI)이다. 주입 받은 정보만 올바르다면 컴포넌트는 항상 올바른 UI를 리턴하게 된다. 로직과 분리되었기 때문에 여러 곳에서 재활용하기도 쉽고, 여러 가지 Container Component와 조합하기도 쉽다.

```text
💡 하지만 Hooks의 등장으로 더이상 이 패턴을 권장하지 않는다.
```

## 2-2. Custom Hook
Custom Hook은 이름이 use로 시작하는 자바스크립트 함수다.
Custom Hook을 사용하면 지금까지 컴포넌트 내부에 한 덩이로 결합하여 사용했던 State와 Effect를 다음과 같이 분리하여 사용할 수 있다. 마치 여러 벽돌을 끼워 맞춰 건물을 만들듯이 React 컴포넌트를 여러 Hook을 조합하는 방식으로 개발할 수 있게 된다.
또한 로직을 독립적인 함수로 분리함으로서 하나의 로직을 여러 곳에서 반복적으로 재사용할 수 있다.

### 여러 기능이 포함된 컴포넌트의 분리
- 사용자에게 input을 받고, 해당 내용을 서버로 전송하는 기능과 화면의 스크롤 높이, 위치 등을 받아오는 기능이 모두 들어있는 경우
```typescript jsx
const UserInput = () => {
	const [size, setSize] = useState({ width: 0, height: 0 })
	const [position, setPosition] = useState({ x: 0, y: 0 })
	const [userInfo, setUserInfo] = useState({
		username: "",
		id: "",
		password: "",
		email: ""
	})

	const handleUserInput = (e) => {
		const { name, value } = e.target
		
		setUserInfo(prev => ({ ...prev, [name]: value }))
	}

	useEffect(() => {
		const handleDocumentSize = () => { ... }

		document.addEventListener("resize", handleDocumentSize)
		return () => {
			document.removeEventListener("resize", handleDocumentSize)
		}
	}, [])

	useEffect(() => {
		const handleDocumentPosition = () => { ... }

		document.addEventListener("resize", handleDocumentPosition)
		return () => {
			document.removeEventListener("resize", handleDocumentPosition)
		}
	}, [])

	return (
		<div>
			<input name="username" onChange={handleUserInput} />
			<input name="id" onChange={handleUserInput} />
			<input name="password" onChange={handleUserInput} />
			<input name="email" onChange={handleUserInput} />
		<div />
	)
}
```

두 가지 Custom Hook으로 나누어 분리해보면, 
1. 먼저 UserInput 에 Logic을 추출하여 View 만 남았고, 
2. 유저 정보와 관련된 useUserInput 과 화면 크기 조절시마다 문서의 크기와 위치를 리턴하는 
useDocumentResize 두 가지 로직으로 분리할 수 있음을 알 수 있다.
```typescript jsx
// hook으로 분리 된 컴포넌트
const UserInput = () => {
	const { userInfo, handleUserInfo } = useUserInput()
	const { size, position } = useDocumentResize()

	return (
		<div>
			<input name="username" onChange={handleUserInput} />
			<input name="username" onChange={handleUserInput} />
			<input name="username" onChange={handleUserInput} />
			<input name="username" onChange={handleUserInput} />
		<div/>
	)
}
```

hook안을 살펴보면,
1. 유저 정보 입력 관련한 로직을 useUserInfo로 분리
```typescript jsx

const useUserInfo = () => {
   const [userInfo, setUserInfo] = useState({
      username: "",
      id: "",
      password: "",
      email: ""
   })

   const handleUserInput = (e) => {
      const { name, value } = e.target

      setUserInfo(prev => ({ ...prev, [name]: value }))
   }

   return { userInfo, handleUserInput }
}
```
2. 화면 리사이즈 관련한 로직을 useDocumentResize 로 관심사에 따라 분리되었다.
```typescript jsx
const useDocumentResize = () => {
	const [size, setSize] = useState({ width: 0, height: 0 })
	const [position, setPosition] = useState({ width: 0, height: 0 })

	useEffect(() => {
		const handleDocumentSize = () => { ... }

		document.addEventListener("resize", handleDocumentSize)
		return () => {
			document.removeEventListener("resize", handleDocumentSize)
		}
	}, [])

	useEffect(() => {
		const handleDocumentPosition = () => { ... }

		document.addEventListener("resize", handleDocumentPosition)
		return () => {
			document.removeEventListener("resize", handleDocumentPosition)
		}
	}, [])

	return { size, position }
}
```

## 2-3. 참고 사항
1. Hook 안에서 다른 Hook을 호출할 수 있다.
> Hook은 State, Effect를 포함할 수 있는 자바스크립트 함수이고, 또 다른 Hook 역시 동일하게 State, Effect를 포함하고 있는 자바스크립트 함수이기 때문에 Hook과 Hook이 중첩 호출될 수 있다.
```typescript jsx
import { useParams } from "react-router-dom"

const useGetUserList = () => {
	const [users, setUsers] = useState([])
    const { id } = useParams()  // 💡

	useEffect(() => {
		fetchUsers(`/users/${id}`)
			.then(res => res.json())
			.then(res => setUsers(res.users))
	}, [])

	return users
}
```
2. 같은 Hook을 사용하는 두 개의 컴포넌트는 state를 공유하지 않는다.
- 두 Custom Hook은 서로 호출되는 위치와 타이밍이 다르며, 
- 애초에 서로 다른 스코프(유효범위)를 생성하기 때문에 
- 컴포넌트를 여러번 호출하는 것처럼 완전히 독립적으로 작동한다.


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

## useReducer

## useSyncExternalStore란?
[useSyncExternalStore](https://beta.reactjs.org/reference/react/useSyncExternalStore)

useSyncExternalStore외부 저장소를 구독할 수 있는 React Hook이다.
```typescript jsx
const snapshot = useSyncExternalStore(subscribe, getSnapshot, getServerSnapshot?)
```
용법
- 외부 스토어 구독
- 브라우저 API 구독
- 커스텀 후크로 로직 추출
- 서버 렌더링 지원 추가

### 관심사 분리의 위력
원리를 명확하게 숙지할 것.

1. 코드의 변경이 어느 지점에서만 일어났는지 확실히 지켜보아야 한다.
2. 그리고 이것이 어떻게 가능했는지 그 의미가 무엇인지 알아야 한다.

핵심
- 상태관리 도구의 변경이 리액트 컴포넌트의 변경을 일으키지 않는다.
- 즉, 관심사의 분리란 서로가 하는 일을 최대한 철저히 모르도록 하여
- 기획이 변경되거나, 미래에 어떤 기능이 추가 되더라도 유연하게 대처할 수 있게 한다.

```text
리액트 컴포넌트에선 변경점이 발생하지 않는걸 발견하셨다면
hook을 쓰는 이유도 알 수 있죠.
상태 관리 도구를 이용하는 지점을 hook으로 격리시켜놨기 때문에
상태관리 도구의 변경이 리액트 컴포넌트의 변경을 일으키지 않는 걸 볼 수 있습니다.
결국 관심사의 분리죠.
```

