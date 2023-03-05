# Parcel-and-ESLint

## Parcel

설정이 딱히 필요없다!

번들러 -> 여러개의 자바스크립트 파일을 하나로 합쳐주는(번들링, 빌드)
최신버전의 자바스크립트를 지원해 주지 않는 브라우저를 위해 옛날버전의 자바스크립트로 변환해주고(babel처럼)

따로 설정이 필요없긴 하지만
다음 두가지 작업을 해주는게 좋다.

`package.json` 파일에 source 속성 추가.

```text
"source": "./index.html",
```

이것을 하지 않고 서버를 실행시키려면 다음으로 실행시켜야 한다.

```shell
npx parcel index.html
```

static 폴더의 파일을 정적 파일로 정적 파일을 Serving 하려면 다음을 셋팅해야 한다.
[parcel-reporter-static-files-copy](https://github.com/elwin013/parcel-reporter-static-files-copy) 패키지 설치 후 `.parcelrc` 파일 작성.

```shell
npm i -D parcel-reporter-static-files-copy
```

```json
{
  "extends": ["@parcel/config-default"],
  "reporters": ["...", "parcel-reporter-static-files-copy"]
}
```

배포버전을 만든다..?
빌드 + 정적 서버 실행

```bash
npx parcel build

npm i servor -D

npx servor ./dist
```

- [servor](https://github.com/lukejacksonn/servor)

### 읽어볼 내용

- [Parcel 공식문서](https://parceljs.org/)
- [Parcel](https://github.com/ahastudio/til/tree/main/parcel)
- [Vite](https://github.com/ahastudio/til/tree/main/vite)

### 추후 정리 내용

- servor

## ESLint

소스코드를 분석해서 프로그램 오류, 버그 등을 잡아준다.

### 정리

기존의 webpack, vite를 사용해 봤는데 configuration이 조금 복잡해서 사용하기 어려웠었다. 그래도 webpack을 사용하면서 살짝 익숙?해져서 vite를 맛봣을 때는 조금 수월했는데 configuration이 없다느 좀..당황?스럽다. 좋은건지..아직 모르겠다.

sorvor와 parcel이 정확이 어떤것들을 해주는지 자세히 알지 못하기 때문에 알게되면 다시 정리하도록!
