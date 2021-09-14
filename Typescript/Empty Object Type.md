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

놀랍게도 [실제로 해보면](https://www.typescriptlang.org/play?#code/JYOwLgpgTgZghgYwgAgILuQbwFDOXAgLiwF8BubE7bBAexAGcxkAPY9VZAXi3yOTBQArinI16TZAE92GHpj5xiAIngMAJvHUMYy5GLqNmAL1md5i4grgaYxAIwAmAMwAWJ65evX+-RSA) 진짜 타입에러가 안난다.

따라서 empty object를 타입으로 쓰고싶으면, 아래처럼 해야한다.

```typescript
type EmptyObject = Record<string, never>; // { [key: string]: never } 랑 똑같다.
```
