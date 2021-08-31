## 목차
- [CORS?](#cors)
- [CORS 허용 헤더](#cors-허용-헤더)
- [Preflight Request](#preflight-request)

## CORS?
domain-a.com에서, domain-b.com으로 요청을 보낸다면?

자신의 origin이 아닌 다른 origin에 요청했으니, 이게 바로 Cross-Origin-Resource-Sharing이다.

origin은 데이터를 가져오는 원천, 즉 도메인이라고 생각하면 된다.

만약 domain-b.com에서 domain-a.com으로부터 오는 요청을 허용하지 않았다면, 브라우저는 CORS에러를 띄우게 된다.

그렇다면 domain-a.com에서 domain-b.com이 자신의 요청을 허용했는지를 어떻게 알 수 있냐? 해당 origin으로 preflight request라는 것을 사전에 보내기 때문이다.

## CORS 허용 헤더
우선 preflight request를 알아보기 전에 CORS를 허용하는 HTTP Response 헤더를 알아야 한다.

클라이언트의 요청에 대해 서버는 "Access-Control-Allow-" 가 앞에 붙는 헤더를 응답함으로써 CORS 허용 여부를 알려주게 된다.

- Access-Control-Allow-Origin
  - 어떤 origin에서 온 요청만 허용할지 결정.
- Access-Control-Allow-Methods
  - 어떤 HTTP Method만 허용할지 결정.
- Access-Control-Allow-Headers
  - 어떤 HTTP 헤더만 허용할지 결정.
- 등등...

## Preflight Request
preflight request는 브라우저가 실제 요청을 보내기 전에, CORS가 허용되었는지 OPTIONS method를 통해 미리 확인하는 요청이다.

우선 브라우저가 prefligh request를 보내지 않는 경우가 있는데, 그러려면 아래의 조건을 모두 충족해야 한다.

- GET, HEAD, POST 중 하나의 method로 요청을 보냈나?
- [여기](https://fetch.spec.whatwg.org/#forbidden-header-name)와 [여기](https://fetch.spec.whatwg.org/#cors-safelisted-request-header)에 있는 HTTP 헤더만 사용했나?

그리고 preflight request를 보냈는지의 여부에 따라 요청 이후 동작이 달라진다.

- 안보낸 경우
  - 실제 요청 -> 응답 받음 -> CORS 허용 헤더 확인
  - 허용된 경우 응답을 보여줌
  - 허용되지 않은 경우 응답을 보여주지 않고 CORS 에러를 뱉음

- 보낸 경우
  - preflight request -> 응답 받음 -> CORS 허용 헤더 확인
  - 허용된 경우 실제 요청을 다시 보냄
  - 허용되지 않은 경우 실제 요청은 보내지 않고 CORS 에러를 뱉음
