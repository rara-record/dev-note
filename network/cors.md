# CORS 

## 학습 키워드 ✏️

- Fetch API 란
- Promise
- ReableStream
- Unicode
- CORS 란

### 동일 출처 정책 (Same Origin Policy)
웹 브라우저가 가지고 있는 보안 정책으로 웹 페이지와 리소스를 요청한 곳,
(현재 프로젝트에서는 해당 REST API SERVER)이 서로 다른 출처 일때(포트까지 포함한다), 
서버에서 얻은 결과를 사용할 수 없게 막는다. 
요청과 응답까지는 이미 다 진행이 된 상황이라는 것을 주의.

## 해결 방법 
### 교차 출처 리소스 공유 (Cross-Origin Resource Sharing, CORS)
CORS를 이용하여 허용한다. 
HTTP헤더를 사용하여, 한 출처에서 실행중인 웹 어플리케이션이
다른 출처의 선택한 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제이다.

REST API 서버에서 Headers에 `Access-Control-Allow-Origin` 속성을 추가하면 된다.
서버에서 `Access-Control-Allow-Origin`를 주고, 웹브라우저가 header를 보고 처리를 하는 것.



> Express에서는 [CORS 미들웨어](https://expressjs.com/en/resources/middleware/cors.html)를 설치해서 사용하면 된다.

```
npm i cors
npm i -D @types/cors
```

추가 코드
```
app.use(cors())
```
