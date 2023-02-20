# Express

최근에 각광받는 프레임워크도 기본으로 Express 지원 할 정도로 
node js로 무엇을 만들때 Express를 먼저 고려한다고 생각할 수 있다.

### 간단한 서버 앱 npm 패키지 세팅
````text
mkdir express-demo-app

ts-node를 쓰는 이유.
타입스크립트로 만든 코드를 노드로 한번 컴파일 해줘야 하는데
tex-node를 쓰면 바로바로 쓸 수 있어서
개발할 때 편하다.

listen
웹브라우저에서 접속할 때 
벋아주는 역할

add.get(/로컬호스트/경로)
경로에는 생략 불가능
req 브라우저에서 요청
res 브라우저로 응답

실행할 때 써 있던거 그대로 가기 때문에ㄷ
다시 시작해야 반영,
코드 고칠때마다
서버를 껐다 켰다 하는 방법
nodemon 개발할때만 쓰면됨. 실제서비스에느 쓰지 않음

## REST API 
HTTP 표준 2000년도

/write-post  xxxx
url에 대해 하나의 리소스만 쓴다
ex) posts --> o 뭔가를한다(crud)

crud에 대해서 http 메소드
Read는 Collection과 item

collection패턴 -> 특정한 패턴이라 추측이 가능

put 전체 덮어씌움
patch 일부 변경(업데이틑) -> 더 많이씀
ex) products{id} => 특정 상푸 정보 변경

put  
patch 
delete 
기본적인 브라우저에서 처리 불가능
featch api, ajax , 등을 통해서 가능

 
