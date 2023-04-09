# React Router

## 학습 키워드

- HTML DOM API
  - Location
  - pathname
- React Router
  - Browser Router
  - Route
  - Memory Router
  - RouterProvider
- Web APIs
  - History
- React Router
  - NavLink
  - Link
  - Navigate
  - useNavigate

## 1. HTML DOM API

하나의 웹 페이지를 하나의 컴포넌트로 만들고,
URL에 따라 적잘한 컴포넌트가 보이게 구현할 수 있다.

[MDN: window.location](https://developer.mozilla.org/ko/docs/Web/API/Window/location)
[MDN: Location](https://developer.mozilla.org/ko/docs/Web/API/Location)

```typescript jsx
function App() {
  const { pathname } = window.location;

  return (
    <div>
      <Header />
      <main>
        {pathname === "/" && <HomePage />}
        {pathname === "/about" && <AboutPage />}
      </main>
      <Footer />
    </div>
  );
}
```

## 2. React Router

> [React Router](https://reactrouter.com/)

### Routes

> [Routes 알아보기](https://reactrouter.com/en/main/components/routes) > [Route 알아보기](https://reactrouter.com/en/main/route/route)

리액트에서 라우터를 쓰는 방법은,
제일 최상위 main 파일에서 `BrowserRouter`로
`<App>` 컴포넌트를 감싸주고, 하위 파일에서 `Routes`, `Route`를 이용하여 해당 URL에 맞는 컴포넌트로 이동할 수 있게 한다.

코드 예시

```typescript tsx
// main.tsx
import { BrowserRouter } from "react-router-dom";

root.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>

  /*
   <BrowserRouter>
      <App />
    </BrowserRouter> 
   한 세트라고 보면 된다.
  */
);
```

```typescript tsx
// App.tsx
import { Routes, Route } from "react-router-dom";

function App() {
  return (
    <div>
      <Header />
      <main>
        <Routes>
          <Route path='/' element={<HomePage />} />
          <Route path='/about' element={<AboutPage />} />
        </Routes>
      </main>
      <Footer />
    </div>
  );
}
```

## 2-1. 라우터 객체를 만들어 쓰는 방법

레이아웃 정보와, 라우팅에 대한 정보를 담은 컴포넌트를 분리해서 사용할 수 있다. `createBrowserRouter`를 사용하면, router 객체를 한묶음으로 만들 수 있다. (이렇게 된다면 사실상 App컴포넌트의 필요성이 없어지게 된다.)

레이아웃 정보에 관해서는, React Router 버전 6.4부터 지원하는 `Outlet`을 사용하여 함께 사용하면, 레이아웃을 계층형으로 볼 수 있어, 분리된 라우터 정보와 함께 사용하기 편하다.

### 먼저 라우터 분리하기

```typescript tsx
import { createBrowserRouter, RouterProvider } from "react-router-dom";

import routes from "./routes";

const router = createBrowserRouter(routes);

root.render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>
);
```

App 컴포넌트를 거치지 않고 바로 브라우저 라우터 만들어서 사용하고, 라우터는 `ReouterProvider`를 이용하여 쓸 수 있다.

> [createBrowserRouter](https://reactrouter.com/en/main/routers/create-browser-router)

> [RouterProvider](https://reactrouter.com/en/main/routers/router-provider)

### Outlet을 사용하여 레이아웃 관리하기

먼저, Layout과 routes 파일을 만든다.

```typescript tsx
import { Outlet } from "react-router-dom";

// Layout.tsx
function Layout() {
  return (
    <div>
      <Header />
      <Outlet />
      <Footer />
    </div>
  );
}

// routes.tsx
const routes = [
  {
    element: <Layout />,
    children: [
      { path: "/", element: <HomePage /> },
      { path: "/about", element: <AboutPage /> },
    ],
  },
];

export default routes;
```

## 3. Link

리액트에서 링크 이동 하는 방법

```typescript tsx
import { Link } from "react-router-dom";
```

```typescript tsx
<Link to='/order'>매장 주문</Link>
```

## 4. MemoryRouter

테스트 환경에서는 MemoryRouter를 쓸 수 있다.
메모리 라우터를 만들어서 테스트하기
| [createMemoryRouter](https://reactrouter.com/en/main/routers/create-memory-router)

```typescript tsx
const context = describe;

describe('App', () => {
 function renderApp(path: string) {
  render((
   <MemoryRouter initialEntries={[path]}>
    <App />
   </MemoryRouter>
  ));
 }

  context('when the current path is '/', () => {
    it('renders the home page', () => {
      renderApp('/');

      screen.getByText(/Welcome/);
    });
  });
});
```

## Test Helper

테스트 코드에서 styled-components의 Theme과 React Router의 Link 등을 사용할 때 문제가 발생하지 않도록, React Testing Library의 render를 한번 감싼 테스트용 헬퍼 함수를 준비.

```typescript tsx
import { render as originalRender } from "@testing-library/react";

import React from "react";

import { MemoryRouter } from "react-router-dom";

import { ThemeProvider } from "styled-components";

import defaultTheme from "./styles/defaultTheme";

export function render(element: React.ReactElement) {
  return originalRender(
    <MemoryRouter initialEntries={["/"]}>
      <ThemeProvider theme={defaultTheme}>{element}</ThemeProvider>
    </MemoryRouter>
  );
}
```

라우터를 객체로 만들어서 쓸때 테스트 코드이다.

```typescript tsx
describe('routes'', () => {
  function renderRouter(path: string) {
		const router = createMemoryRouter(routes, { initialEntries: [path] });
		render(<RouterProvider router={router} />);
	}

	context('when the current path is “/”', () => {
		it('renders the home page', () => {
			renderRouter('/');

			screen.getByText(/Hello/);
		});
	});

	context('when the current path is “/about”', () => {
		it('renders the about page', () => {
			renderRouter('/about');

			screen.getByText(/About/);
		});
	});
});

```
