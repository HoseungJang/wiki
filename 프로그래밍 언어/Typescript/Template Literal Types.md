Typescript 4.1부터 Template Literal Types 기능이 추가되었다.

쉽게 말하면, Javascript의 Template Literal 문법과 Typescript의 String Literal Types 문법이 합쳐진 거라고 보면 된다.

아래와 같이 사용한다.

```typescript
type FirstName = "호승";
type Name = `장${FirstName}`; // "장호승"
```

Union도 적용 가능하다.

```typescript
type FirstName = "호승" | "래원";
type Name = `장${FirstName}`; // "장호승" | "장래원"
```

또한, infer 키워드를 사용해 추론까지 할 수 있다.

```typescript
type ExtractFirstName<T extends string> = T extends `장${infer FirstName}` ? FirstName : never;
type FirstName = ExtractFirstName<"장호승">; // "호승"
```

이걸 활용하면 기존에 불가피하게 Type-safe하지 못했던 문제들을 해결할 수 있다.

- [document.querySelector를 Type-safe하게 사용](https://twitter.com/MikeRyanDev/status/1308472279010025477)
- [express의 path parameters 추론](https://twitter.com/danvdk/status/1301707026507198464)
