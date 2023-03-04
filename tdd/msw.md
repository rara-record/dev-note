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
브라우저나 노드에서 쓸 수 있는 네트워크 레벨에서 프록시를 이용해서 가짜로 구현해준다.

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
src/setupTests.ts 파일
```javascript

import server from './mocks/server';

beforeAll(() => server.listen({ onUnhandledRequest: 'error' }));

afterAll(() => server.close());

afterEach(() => server.resetHandlers());
```

fetch가 브라우저에서는 되는데, 최신 노드가 아닌 이상 안된다.
github fetchpolyfill
