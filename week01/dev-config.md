# 개발 환경 세팅

개발환경에 관련된 도구가 계속 나오는데 내가 현재 하는 것에 있어서 어떤것이 유리한지 파악하고 사용할 수 있게 되는 것이 좋다. 즉 현재 개발환경에 대해 어떤 도구를 사용하든 그것으로 끝이 아니라 계속 다른 방향, 다른 도구로 업데이트 될 수 있으니 계속 새로운 것에 적응하고 사용하려는 자세가 필요하다.

우선 자바스크립트 개발 환경을 세팅하기 위해 node.js를 설치해야 하는데 fnm이라는 툴을 사용해서 버전관리를 한다. fnm(fast node manager)를 사용하는 이유는 프로젝트마다 버전이 다를 수 있기 때문에 여러버전을 설치해서 프로젝트마다 버전을 변경할 수 있게 해준다.

나는 Mac을 사용하기 때문에 homebrew를 이용해 fnm을 설치할 수 있다.

```shell
brew install fnm
```

### Node.js설치

설치 가능한 버전 보기

```shell
fnm ls-remote
```

LTS(Long Term Support) 버전을 설치하고 기본으로 사용하게 한다. LTS는 가장 최신 버전은 아니지만 나온 버전중에 가장 안정화되어 있는 최신버전이다.

```shell
fnm install --lts
fnm use lts-latest
fnm default $(fnm current)
```

설치된 버전 보기

```shell
fnm list
fnm current
```

작업폴더를 생성하자

```shell
mkdir my-app
```

기본으로 package.json 파일을 설치해준다.

```shell
npm init -y
```

간단하게 package.json파일을 수정하고 나서
.gitignore 파일을 만들어준다. 이 파일은 git에 올릴때 파일이나 폴더를 제외시킬 수 있다.

[gitignore](https://www.toptal.com/developers/gitignore) 사이트를 들어가면 .gitignore 파일을 설정할 수 있게 해준다.

타입스크립트 설정을 해보자
-D로 설치하면 개발환경을 위한 패키지로 devDependency로 설치 된다.

```shell
npm i -D typecsript
npx tsc --init
```

설치하고 실행하면 tsconfig.json파일이 생성된다.
우선 그 파일에서 해당 부분을 주석에서 살리고 속성을 변경해주자

```json
"jsx": "react-jsx"
```

ESLint 설정

```shell
npm i -D eslint
npx eslint --init
```

실행하고 나면 npm init처럼 고르는 것이 나오는데

1. To check syntax, find problems, and enforce code style
2. JavaScript modules (import/export)
3. 원하는 프로젝트 타입 선택
4. 타입스크립트 사용 여부
5. Browser
6. Use a popular style guide
7. XO: https://github.com/xojs/eslint-config-xo-typescript
8. JavaScript

으로 설정하면 된다. 그러고나면 .eslintrc.js라는 파일이 생성되는데 다음을 추가하자

```js
    env: {
		browser: true,
		es2021: true,
		jest: true // jest 추가
	},
```

또 .eslintignore 파일을 작성하자

```
/node_modules/
/dist/
/.parcel-cache/
```

설정하고 나서 코드를 작성할때 eslint에 맞지 않으면 빨간줄이 뜰텐데 고치고 싶으면 다음을 입력하자

```
npm eslint --fix .
```

추가적으로 나는 eslint를 airbnb스타일로 적용하고 싶어서 다음을 참고했다
(https://poiemaweb.com/eslint)

리액트 설치

```
npm i react react-dom
// 타입스크립트 설정
npm i -D @types/react @types/react-dom
```

테스팅 도구 설치

```
npm i -D jest @types/jest @swc/core @swc/jest \
    jest-environment-jsdom \
    @testing-library/react @testing-library/jest-dom
```

jest와 swc를 사용하기 위해 설치하는 것이고 추후에 어떤것들이 변경되어도 당황하지말고 공식문서를 찾아가며 설정해보자.

jest.config.js 파일을 설치하고

```js
module.exports = {
  testEnvironment: 'jsdom',
  setupFilesAfterEnv: ['@testing-library/jest-dom/extend-expect'],
  transform: {
    '^.+\\.(t|j)sx?$': [
      '@swc/jest',
      {
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
      },
    ],
  },
  testPathIgnorePatterns: ['<rootDir>/node_modules/', '<rootDir>/dist/'],
};
```

parcel 설치
dev서버를 띄울 수 있다.

```
npm i -D parcel
```

이제 필요한 설치는 어느정도 끝났고
package.json파일의 source, scripts 부분을 수정해 필요한 실행 명령어들을 설정하자

```json
"source": "index.html",
"scripts": {
    "start": "parcel --port 8080",
    "build": "parcel build",
    "check": "tsc --noEmit",
    "lint": "eslint --fix --ext .js,.jsx,.ts,.tsx .",
    "test": "jest",
    "coverage": "jest --coverage --coverage-reporters html",
    "watch:test": "jest --watchAll"
  },
```

source부분은 parcel로 웹서버를 띄울때 타겟이 되는 파일경로를 적어주면 된다.

설정이 완료됐으면 기본 파일들을 생성해보자

```html
//index.html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <title>React Demo App</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="./src/main.tsx"></script>
  </body>
</html>
```

```ts
//src/main.tsx
import ReactDOM from 'react-dom/client';

const App = () => <p>Hello, world</p>;

const element = document.getElementById('root');

if (element) {
  const root = ReactDOM.createRoot(element);
  root.render(<App />);
}
```
