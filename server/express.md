# Express

## 학습 키워드  ✏️

- Express 란
- URL 구조
- REST API
- HTTP Method(CRUD)

최근에 각광받는 프레임워크도 기본으로 Express 지원 할 정도로 
많은 프레임워크들이 Express를 기반으로 하고 있다.

### 간단한 서버 앱 npm 패키지 세팅

[Express 설치](https://expressjs.com/ko/starter/installing.html)

typescript
```Bash
npm i -D typescript
npx tsc --init
```

ESLint
```Bash
npm i -D eslint
npx eslint --init
```

ts-node 
```Bash
npm i -D ts-node
````
> ts-node를 쓰는 이유: 
> 타입스크립트로 만든 코드를 노드로 한번 컴파일 해줘야 하는데 ts-node를 쓰면 바로바로 쓸 수 있어서
개발할 때 편하다.

app.ts 파일 작성
```typescript
import express from 'express';

const port = 3000;

const app = express();

app.get('/', (req, res) => {
	res.send('Hello, world!');
});

app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});
```
### app.get / app.listen
**라우팅** : 라우팅은 어플리케이션 엔드 포인트(URI)의 정의, 
> 즉, URI가 클라이언트 요청에 응답하는 방식이다.

라우트 메소드
Express는 HTTP 메소드에 get, post, put, head, delete, options.. 
등등과 같은 라우팅 메소드를 지원한다.

매우 기본적인 라우트의 예
```
add.get(/로컬호스트/경로, (req, res) => { })
```
경로에는 생략 불가능하며, 
- req : 브라우저에서 요청
- res :  브라우저로 응답

```text
app.listen([port[, host[, backlog]]][, callback])
app.listen(서버를 오픈할 포트번호, () => { 서버 오픈시 실행할 코드 })
```
> app.listen()은 원하는 포트에 서버를 오픈하는 문법으로,
listen() 함수 안엔 두개의 파라미터가 필요하다.

### 서버 실행
```
npx ts-node app.ts
```

코드를 고칠때마다 서버를 재실행하는 문제가 있어서 nodemon을 사용한다.
nodemon 개발할때만 쓰면되고, 실제 서비스에는 쓰지 않는다.

```Bash
npx nodemon app.ts
```


## REST API 
HTTP 표준 2000년도

```text
http://localhost:8080/post
url/resource
```
url에 대해 하나의 리소스만 쓴다
````text
/write-post  // 이렇게 하지 않고
/post   // 이렇게
````

crud에 대해서 http 메소드 
Read는 Collection과 item

collection패턴 -> 특정한 형태로 추측이 가능해서 씀

put 전체 덮어씌움
patch 일부 변경(업데이틑) -> 더 많이씀
ex) products{id} => 특정 상푸 정보 변경

put  
patch 
delete 
기본적인 브라우저에서 처리 불가능
featch api, ajax , 등을 통해서 가능

 
### 예제
```typescript 
app.get('/products', (req, res) => {
    const products = [
        {
            category: 'Fruits', price: '$1', stocked: true, name: 'Apple',
        },
        {
            category: 'Fruits', price: '$1', stocked: true, name: 'Dragonfruit',
        },
        {
            category: 'Fruits', price: '$2', stocked: false, name: 'Passionfruit',
        },
        {
            category: 'Vegetables', price: '$2', stocked: true, name: 'Spinach',
        },
        {
            category: 'Vegetables', price: '$4', stocked: false, name: 'Pumpkin',
        },
        {
            category: 'Vegetables', price: '$1', stocked: true, name: 'Peas',
        },
    ];
    
    res.send({ products });
});

```
