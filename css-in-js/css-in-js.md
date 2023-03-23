# CSS in JS

## 학습 키워드

- css in js란?
- css in js의 성능이슈
- Styled-components
- Emotion

## 1. CSS

필수 개념 레퍼런스:

- [FlexBox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)
- [Grid](https://css-tricks.com/snippets/css/complete-guide-grid/)

## 2. CSS In JS를 쓰는 이유

1. **컴포넌트로 사고할 수 있다는 점**

## 성능 이슈

> [CSS-in-JS와 성능 (2021)](https://hyeonseok.com/blog/877)

→ CSS 파일과 JS 파일 로딩의 차이.

> [Why We're Breaking Up with CSS-in-JS (2022)](https://bit.ly/3g6QufF)

## 3. Styled-components

> 유명한 라이브러리들은 대부분 ‘왜’ 만들어졌는지 혹은 품고 있는 철학 등이 잘 정리되어 있는 경우가 많습니다. 라이브러리를 사용할 때는 공식문서를 꼭 살펴보는 습관을 하시는 게 좋습니다.
>
> 참고 레퍼런스: https://styled-components.com/docs/basics

스타일이 적용된 컴포넌트를 쉽게 만들 수 있는 도구.

반드시 VS Code Extension을 설치해서 쓰자. 도구에서 제대로 지원 안 하면 사람이 쓸 게 못 됨.

Babel Plugin을 SWC에서 쓸 수 있도록 포팅한 것도 함께 설치하자(SSR 지원 등을 위한 공식 권장사항).

```text
npm i styled-components
npm i -D @types/styled-components @swc/plugin-styled-components
```

`.swcrc` 파일 작성하기

```json
{
  "jsc": {
    "experimental": {
      "plugins": [
        [
          "@swc/plugin-styled-components",
          {
            "displayName": true,
            "ssr": true
          }
        ]
      ]
    }
  }
}
```

### 3-1. 스타일 추가하기

1. 추가로 스타일을 입힐 때

```typescript tsx
import styled from "styled-components";

function HelloWorld({ className }: React.HTMLAttributes<HTMLElement>) {
  return <p className={className}>Hello, world!</p>;
}

const Greeting = styled(HelloWorld)`
  color: #00f;
`;

export default Greeting;
```

### 3-2. 기존 컴포넌트에 스타일을 입힐 때

기존 컴포넌트에 스타일을 입히는 것도 가능하지만,
기존 컴포넌트가 Class를 잡아주어야 한다.

```typescript tsx
import styled from "styled-components";

function HelloWorld({ className }: React.HTMLAttributes<HTMLElement>) {
  return <p className={className}>Hello, world!</p>;
}

const Greeting = styled(HelloWorld)`
  color: #00f;
`;

export default Greeting;
```

### 3-3. Props 활용

> [Passed props](https://styled-components.com/docs/basics#passed-props)

1. 활성화 여부 표현 할 때.
2. 특정 스타일을 잡아주고 싶을 때.

```typescript tsx
import { useBoolean } from "usehooks-ts";

import styled, { css } from "styled-components";

type ParagraphProps = {
  active?: boolean;
};

const Paragraph = styled.p<ParagraphProps>`
  color: ${(props) => (props.active ? "#F00" : "#888")};

  ${(props) =>
    props.active &&
    css`
      font-weight: bold;
    `}
`;

export default function Greeting() {
  const { value: active, toggle } = useBoolean(false);

  return (
    <div>
      <Paragraph>Inactive</Paragraph>
      <Paragraph active>Active</Paragraph>
      <Paragraph active={active}>
        Hello, world{" "}
        <button type='button' onClick={toggle}>
          Toggle
        </button>
      </Paragraph>
    </div>
  );
}
```

### 3-4. 속성 추가 (attrs)

1. 기본 속성을 추가할 수 있다.
2. 불필요하게 반복되는 속성을 처리할 때 유용한데, 버튼 등을 만들 때 적극 활용한다.

```typescript tex
import styled form 'styled-component';

type ButtonProps = {
  active: boolean
}

const Button = styled.button.attrs({
  type: 'button', // 'submit' ,, 'reset'
})<ButtonProps>`
  border: 1px solid #fff;
  color: ${(props) => (props.active ? '#F00' : '#888')};

  ${(props) => props.active && css`
    font-weight: bold;
  `}
`;


export default Button;
```

버튼의 타입을 유동적으로 바꾸고 싶다면?

```typescript tsx
type ButtonProps = {
  type: "button" | "submit" | "reset"; // 타입 추가
  active: boolean;
};

const Button = styled.button.attrs<ButtonProps>((props) => ({
  type: props.type ?? "button", // 기본값 설정
}))<ButtonProps>`
  border: 1px solid #fff;
  color: ${(props) => (props.active ? "#F00" : "#888")};

  ${(props) =>
    props.active &&
    css`
      font-weight: bold;
    `}
`;
```

## 4. Emotion 정리

### 4-1. base

```text
npm i @emotion/css
```

서버 사이드 렌더링 설정

> <https://emotion.sh/docs/ssr#api>

### API

1. CSS

- 템플릿 리터럴, 객체 또는 객체 배열로 받아들이고 클래스 이름을 반환한다.

> <https://emotion.sh/docs/@emotion/css#animation-keyframes>

### CX CLASS

클래스 이름 결합

```typescript tsx
import { cx, css } from '@emotion/css'

const cls1 = css`
  font-size: 20px;
  background: green;
`
const cls2 = css`
  font-size: 20px;
  background: blue;
`

<div className={cx(cls1, cls2)} />

```

조건부 클래스 이름

```typescript tsx
const cls1 = css`
  font-size: 20px;
  background: green;
`
const cls2 = css`
  font-size: 20px;
  background: blue;
`

const foo = true
const bar = false


<div
  className={cx(
    { [cls1]: foo },
    { [cls2]: bar }
  )}
/>

```

다른 소스의 클래스 이름을 사용 할 수도 있다.

```typescript tsx

const cls1 = css`
  font-size: 20px;
  background: green;
`

<div
  className={cx(cls1, 'profile')}
/>

```
