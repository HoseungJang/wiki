타입스크립트에서 object의 key들 중 특정 조건을 만족하는 것들만 허용하고 싶은 경우가 있다.

예시를 하나 들어보자.

```typescript
const obj = {
  a: 123,
  b: "string1",
  c: "string2",
};

function getValueFromObj(key: string) {
  return obj[key] ?? null;
}
```

여기서 obj의 key들 중 string value를 가진 것들(위 예제에선 b, c)만 받을 수 있게 하려면 어떻게 해야할까?

아래와 같이 3항식 문법을 사용해서 조건에 맞는 keys만 추출할 수 있다.

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
  return obj[key];
}
```

extends로 string value를 가진 것들만 필터링했고, 더이상 function의 return type도 nullable하지 않다.
