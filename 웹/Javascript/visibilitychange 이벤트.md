> 참고: [Page Visibility API](https://www.w3.org/TR/page-visibility/)

document에는 visibilityState라는 속성이 있다. "visible" 또는 "hidden" 중 하나가 담겨있는데, 이를 통해 사용자가 현재 페이지를 보고있는지 알아낼 수 있다. "visible"일 때가 사용자가 보고있음을 의미한다.

또는 boolean 값이 들어있는 document.hidden으로도 visibilityState를 판별할 수 있다. document.hidden === true인 경우, visibilityState는 "hidden"이다.

visibilityState의 변화는 visibilitychange 이벤트로 확인할 수 있는데, [호환성](https://caniuse.com/?search=visibilitychange)이 나쁘지 않다.

```typescript
const handler = () => {
  if (document.visibilityState === "visible") {
    /* ... */
  }
}
document.addEventListener("visibilitychange", handler, { once: true });
```
