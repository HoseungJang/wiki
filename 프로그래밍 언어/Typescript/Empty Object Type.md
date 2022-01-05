타입스크립트에서 빈 객체를 타입으로 어떻게 정의할 수 있을까?

일반적으로 아래와 같이 정의하면 될거라고 생각할 것이다.

```typescript
type A = {};
interface A {}
```

하지만 typescript에서 저 타입은 nullish하지 않은 그 어떤 값도 허용한다는걸 의미한다.

예를들어, 아래처럼 코드를 짜도 문제가 없다는 말이다.

```typescript
interface AAA {
  aaa: {};
}

const x: AAA = { aaa: true };
const y: AAA = { aaa: "fasdfadsf" };
const z: AAA = { aaa: { asdf: 12341242344 } };
```

[Typescript Playground](https://www.typescriptlang.org/play?#code/JYOwLgpgTgZghgYwgAgILuQbwFDOXAgLiwF8BubE7bBAexAGcxkAPY9VZAXi3yOTBQArinI16TZAE92GHpj5xiAIngMAJvHUMYy5GLqNmAL1md5i4grgaYxAIwAmAMwAWJ65evX+-RSA)

(놀랍게도 진짜 타입에러가 안난다.)

따라서 empty object를 타입으로 쓰고싶으면, 아래처럼 해야한다.

```typescript
type EmptyObject = Record<string, never>; // { [key: string]: never } 랑 똑같다.
```

단, intersection type에선 {}가 "nullish하지 않은 그 어떤 값도 허용"을 의미하지 않는다.


```typescript
const a: {} & true = true;
const b: {} & true = {}; // 타입 에러
const c: {} & { asdf: number } = {};
const d: {} & { asdf: number } = { asdf: 1234 };
const e: {} & { asdf: number } = { qwer: "asdf" }; // 타입 에러
```

[Typescript Playground](https://www.typescriptlang.org/play?#code/MYewdgzgLgBAhgLhgbwL4wGQygJwK4CmMAvNvgQNwBQoksARkmpmYSSqhTAPTcyADC4FDxmIAXRwDftNcNBjAm6LMngQAJgDMkYPAFt6BHDHSk0XXgOHjJdGMrktFcFepiadeg+3uOkARgBMAZgAWA2paaQJbBSU1DW1dfUMUGABHAHc9JAAiBzVMkJ4+IVEJIA)

따라서 특정 상황에서는 {}를 써도 문제되지 않는다. (예를 들면 React props)

```typescript
type Props = React.PropsWithChildren<{}>; // { children?: ReactNode }
```
