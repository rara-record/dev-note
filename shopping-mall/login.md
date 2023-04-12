# Login

로그인은 프론트엔드 개발자 입장에서, 사용자의 정보를 백엔드에 전송 후 ACCESS_TOKEN을 얻는 과정이다.

## 전반적인 로직

전반적인 로직은,
로그인 성공시 : ACCESS_TOKEN이 있다면 로그인이 됐다는 말이므로 홈으로 리다이렉션 시킨다.
로그인 실패시 : 에러코드가 내려오면, 에러 팝업을 띄워주거나 문구를 보여주면 될 것 같다.

로그인 스토어에는, 에러 처리나 서버에 요청 할 값과
ACCESS_TOKEN의 여부, 그리고 유효성 검사도 해준다.

> 초기값 예시

```typescript
email: '',
password: '',
error: false,
accessToken: '',
```

로그인에 관한 스토어를 만든 후에, api를 호출한다!
`LoginForm` 컴포넌트에서 스토어 부른 후,
ACCESS_TOKEN등 필요한 데이터를 가져온다.
만약 ACCESS_TOKEN이 있다면, 로컬 스토리지에 저장해주는데
이때, 로컬 스토리지는 hook안에 넣어 추상화를 시킨다.

ACCESS_TOKEN을 두 곳에서 관리하고 있는데,
store와 토큰을 관리하는 로컬 스토리지를 추상화 시킨 hooks이다. store에서는, 백엔드에서 받아온 token을 가지고 있고, `LoginForm`에서는 받아온 token을 로컬스토리지에
저장한다.

로컬 스토리지를 hook에 추상화 시킨 이유는,
store는 리액트 바깥에 있는 외부 저장소이고
(external-store), hook에서 직접 스토어를 연결하기 보다는
스토어 - 컴포넌트 - 훅을 연결해서, 컴포넌트가 그 연결 고리 역할을 하는 것을 기대하는 것이라고 이해하고 있다.

> 💡 로그인 페이지 진입 시, store 리셋 함수 호출 필수!

## 로그인 후 원래 있던 위치 기억하여, 리다이렉트하기!

```typescript tsx
import { Link } from "react-router-dom";

function MyComponent() {
  const location = useLocation();
  const redirectTo = location.pathname;

  return (
    <div>
      <Link to={`/login?redirect_to=${redirectTo}`}>로그인을 하거라</Link>
    </div>
  );
}
```
