타입스크립트에서 object의 key들 중 특정 조건을 만족하는 것들만 허용하고 싶은 경우가 있다.

예를 들면 아래와 같은 경우이다.

```typescript
const obj = {
  a: 123,
  b: "string1",
  c: "string2",
};

function getStringValueFromObj(key: string) {
  return obj[key] ?? null;
}
```

위 예제는 return value가 nullable하다.

그런데 애초에 obj의 key인 a, b, c 중 string 값을 갖고 있는 b, c만 인자로 받을 수 있다면 어떨까?

아래와 같이 extends와 3항식을 사용해서 조건에 맞는 keys만 추출할 수 있다.

```typescript
const obj = {
  a: 123,
  b: "string1",
  c: "string2",
};

type Keys = keyof typeof obj;

// "b" | "c"
type StringValueKeys = {
  [K in Keys]: (typeof obj)[K] extends string ? K : never;
}[Keys];

function getStringValueFromObj(key: StringValueKeys) {
  return obj[key] ?? null;
}
```
