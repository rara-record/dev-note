# Playwright

## 학습 키워드 ✏️

- E2E(End to End) Test
- Headless Chrome
- Puppeteer
- Playwright
- CodeceptJS

## E2E테스트란?

[카카오 기술 블로그 E2E 테스트 도입 경험기](https://fe-developers.kakaoent.com/2023/230209-e2e/)

E2E 테스트는 End To End 테스트의 약자로 애플리케이션의 흐름을 처음부터 끝까지 테스트하는 것을 의미한다.

## playwright

웹 브라우저 기반 E2E 테스트 자동화 도구.
Headless Chrome을 기반으로 한 Puppeteer를 계승하면서, 더 많은 웹 브라우저를 지원한다.

```text
npm i -D @playwright/test eslint-plugin-playwright
```

playwright.config.ts 파일

```javascript
import { PlaywrightTestConfig } from "@playwright/test";

const config: PlaywrightTestConfig = {
  testDir: "./tests",
  retries: 0,
  use: {
    channels: "chrome", // add browser channels
    baseURL: "http://localhost:8080",
    headless: !!process.env.CI,
    screenshot: "only-on-failure",
  },
};

export default config;
```
