> 참고: [Page Visibility API](https://www.w3.org/TR/page-visibility/)

document에는 visibilityState가 있고, "visible", "hidden" 중 하나의 값이 담겨있고, boolean 값이 담겨있는 document.hidden으로도 visible 여부를 확인할 수 있다.

그리고 visibilityState의 변화는 visibilitychange 이벤트로 확인할 수 있는데, [호환성](https://caniuse.com/?search=visibilitychange)이 나쁘지 않다.

```typescript
const handler = () => {
  if (document.visibilityState === "visible") {
    /* ... */
  }
}
document.addEventListener("visibilitychange", handler, { once: true });
```
