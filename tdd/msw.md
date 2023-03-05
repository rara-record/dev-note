# MSW

## 학습 키워드 ✏️

- Service worker
- MSW(Mock Service Worker)
- polyfill(폴리필)

## 1. Service worker
ServiceWorker는 
1. 웹 서비스에서도 백그라운드 동기화, 푸시 알림 등이 가능하도록 지원해주는 도구이다.
2. 웹 서비스와 브라우저 및 네트워크 사이에서 프록시 서버의 역할을 하여, 
오프라인에서도 서비스를 사용할 수 있도록 한다.
4. 웹 페이지와 별개로 존재하므로, DOM이나 window 요소에 접근 할 수 없으며,
5. 요청하지 않는 이상, 없는 것이다 다름이 없다.

### 캐시와 상호 작용
`fetch` 이벤트의 중간자 역할로 사용할 수 있다.
서비스워커는 HTTP를 통해 정보를 요청하는 대신 가지고 있는 캐시에서 자료를 전달합니다. 
캐시가 삭제되지 않는 한 브라우저는 인터넷 연결 없이도 정보를 보여줄 수 있다.


## 2. MSW
> [MSW](https://mswjs.io/)
>

> [Service Worker API](https://developer.mozilla.org/ko/docs/Web/API/Service_Worker_API)
>

> [아샬의 Mock Service Worker (MSW)](https://github.com/ahastudio/til/blob/main/mock-api/msw.md)
>

> [Mocking REST API](https://mswjs.io/docs/getting-started/mocks/rest-api)
>

> [Integrate mocking into Node](https://mswjs.io/docs/getting-started/integrate/node)
>
 
테스트 코드 Mocking을 이용하여 코드 레벨에서 가짜 구현을 할 수 있다면,
MSW를 이용하면 Mock 서버를 구축하지 않아도 네트워크 레벨에서 프록시를 이용하여 가짜 구현이 가능하다.

다시 말해, 네트워크 호출을 가로채는 녀석인데 
백엔드 API인척 하며 프론트엔드 요청에 가짜 데이터를 응답해준다.

### 사용하는 이유는 무엇인가
백앤드 API 개발과 프런트앤드 UI 개발이 동시에 진행되야하는 경우, 백앤드 API 구현이 완료될 때까지 프런트앤드 팀에서 임시로 사용하기 위한 가짜(mock) API를 서비스 워커로 돌리기 위해서이고, 
두번째는 테스트를 실행 시 실제 백앤드 API에 네트워크 호출을 하는 대신에 훨씬 빠르고 안정적인 가짜 API 서버를 구축하기 위해서다.

MSW 패키지 설치
```
npm i -D msw
```

jest.config.js 파일의 “setupFilesAfterEnv” 속성에 setupTests.ts 파일 추가.
```javascript
module.exports = {
	testEnvironment: 'jsdom',
	setupFilesAfterEnv: [
		'@testing-library/jest-dom/extend-expect',
		'<rootDir>/src/setupTests.ts',
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

## 2-1. 요청 핸들러 작성
가짜 API를 구현하려면 요청이 들어왔을 때 임의의 응답을 해주는 핸들러(handler) 코드를 작성해야한다.
모킹 관련 코드를 프로젝트의 아무데나 상관해도 상관은 없으나 
mocks이라는 디렉토리에 두는 것이 일반적인 관례이다.

REST API를 모킹할 때는 msw 모듈의 rest 객체를 사용한다.
Express.js 서버에서 볼 수 있는 코딩 패턴과 상당히 유사한 방식으로 핸들러를 구현할 수 있습다.

```typescript
// src/mocks/handlers.ts 파일

import { rest } from "msw";
import fixtures from "../../fixtures";

const BASE_URL = process.env.API_BASE_URL || "http://localhost:3000";

const handlers = [
  rest.get(`${BASE_URL}/products`, (req, res, ctx) => {
    const { products } = fixtures;

    // ctx 뒤에는 세부 내용이 들어간다. status(200)은 default다. 생략 가능
    return res(ctx.status(200), ctx.json({ products }));
  }),
];
export default handlers;
```

## 2-2. 모의 서버 설정
다음으로 msw 모듈에서 제공하는 setupWorker() 함수를 사용해서 서비스 워커를 생성한다. 
위에서 작성한 요청 핸들러 코드를 불러와서 그대로 setupWorker() 함수의 인자로 넘겨주면 된다.

```typescript
// src/mocks/server.ts

import { setupServer } from 'msw/node';

import handlers from './handlers';

const server = setupServer(...handlers);

export default server;
```

## 2-3. setup 파일 설정
src/setupTests.ts 파일
```javascript
// polyfill library, 뒤에 내용에서 다룸
import "whatwg-fetch";

import server from './mocks/server';

beforeAll(() => server.listen({ onUnhandledRequest: 'error' }));

afterAll(() => server.close());

afterEach(() => server.resetHandlers());
```

### 실행
```typescript jsx
// App.test.ts
import { render, screen, waitFor } from '@testing-library/react';

import App from './App';

// jest.mock 불필요.

test('App', async () => {
	render(<App />);
	
	await waitFor(() => { // waitFor: 콜백함수 안에 있는 것이 될때까지 확인함.
		screen.getByText('Apple');
	});
});
```

## 3. polyfill(폴리필) 
fetch API가 브라우저에서는 되는데, 노드에서는 안된다. (최신 노드에서는 반영됨)
그래서 노드에서 fetch를 쓸 수 있도록
github에서 만든 fetch polyfill 라이브러리를 가져다 쓸 수 있다.

설치
```text
npm i -D whatwg-fetch
```
작성했던 setup 파일 맨 위에 불러와준다.
src/setupTests.ts 파일
```javascript
// 추가
import "whatwg-fetch";

import server from './mocks/server';

beforeAll(() => server.listen({ onUnhandledRequest: 'error' }));

afterAll(() => server.close());

afterEach(() => server.resetHandlers());
```
