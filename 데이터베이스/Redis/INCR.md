Redis에서 INCR은 number value를 1 증가시켜주는 command이다.

```typescript
redis.set("key", 0);
redis.client.incr("key");
redis.get("key"); // 1
```

ICNR을 시도했을 때 key에 매핑된 value가 없는 경우, 내부적으로 해당 key의 value를 0으로 초기화한 후 INCR을 실행한다.

INCR은 counter, rate limiter 패턴 등에 간편하게 사용할 수 있다.

페이지 방문자 수를 측정하거나(counter), IP 별로 일일 API 사용 횟수를 체크하고 이용을 제한하는(rate limiter) 정도로 예시를 들 수 있겠다.
