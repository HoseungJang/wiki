## Strict-Transport-Security란?
HTTP Strict-Transport-Security(이하 HSTS)는 웹 사이트가 웹 브라우저에게 자신과 HTTP가 아닌 HTTPS로만 통신하라고 알리는 HTTP Response Header이다.

### 사용법
```
Strict-Transport-Security: max-age=<expire-time>
Strict-Transport-Security: max-age=<expire-time>; includeSubDomains
Strict-Transport-Security: max-age=<expire-time>; preload
```

- ```max-age=<expire-time>```: HTTPS로만 접속해야 함을 강제해둘 시간
- ```includeSubDomains```: 이 사이트의 모든 서브도메인에 HSTS를 적용할 것인지
- ```preload```: HSTS preload list에 등록할 것인지

## 시나리오
### 해커에게 어떻게 당하는가
1. 최초로 HTTP로 접속하면, 웹 서버가 302 Redirect 응답으로 HTTPS로 리다이렉트를 시도한다.
2. 해커가 해당 302 Redirect 응답을 가로채서 가짜 사이트로 리다이렉트 시켰다. (이런 공격을 MITM(Man In The Middle)이라고 함)
3. 해커는 신나게 당신의 정보를 훔쳐간다.

### HSTS를 사용한다면
1. 당신이 이미 HTTPS로 사이트에 접속한 적이 있었다면,
2. 당신의 브라우저는 그것을 기억하고 HTTP가 아닌 HTTPS만 사용할 것이다.
3. 따라서 해커의 MITM 공격을 방지할 수 있다.

HTTP의 경우 Strict-Transport-Security 헤더가 무시된다. 해커가 응답을 가로채서 변조했을 가능성이 있기 때문이다.

즉, HSTS를 사용하는 사이트에 HTTPS를 통해 최초 1회는 접속한 적이 있어야 브라우저가 해당 사이트에 대해 HTTPS-only로 통신한다는 것이다.

결국 최초 접속시 공격당할 위험을 배제할 수 없다는 것인데, 이런 경우를 위해서 대부분의 브라우저에서는 HSTS preload list에 웹 사이트를 미리 등록해놓을 수 있다.

## HSTS preload list
HSTS preload list는 구글 Chrome 브라우저의 내 웹사이트를 HTTPS-only로 요청하라고 미리 등록해놓을 수 있는 기능이다.

실제로 IE, Firefox 등 여러 유명 브라우저들은 Chrome의 preload list에 기반한 HSTS preload list를 사용하고 있다.

[여기](https://caniuse.com/stricttransportsecurity)에서 HSTS를 지원하는 브라우저를 볼 수 있다.

여담으로 Chrome 브라우저를 사용하고 있는 경우, 주소창에 chrome://net-internals/#hsts를 입력하면 HSTS preload list를 직접 만져볼 수 있다.
