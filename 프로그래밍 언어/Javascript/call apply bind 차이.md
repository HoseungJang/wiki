자바스크립트 Function 객체의 프로토타입에는 call, apply, bind 메소드가 있다. 따라서 모든 함수는 그걸 사용할 수 있다.

각각의 메소드의 공통점은 함수의 호출 방식과 상관없이 this를 원하는 값으로 바인딩할 수 있다는 점이다.

그렇다면 세 메소드의 차이는 무엇일까?

## call
call은 함수에 바인딩시킬 this값과, 파라미터로 넘어갈 인자값들을 명시적으로 받고 함수를 실행한다.

```javascript
function f(a, b) {
  console.log(this);
  console.log(a + b);
}
f.call({ asdf: 1234 }, 1, 2) // { asdf: 1234 }, 3
```

## apply
apply는 call과 같이 this와 인자값을 받아 함수를 실행하지만, 인자값을 배열로 받는다.

```javascript
function f(a, b) {
  console.log(this);
  console.log(a + b);
}
f.apply({ asdf: 1234 }, [1, 2]) // { asdf: 1234 }, 3
```

이건 그냥 팁인데, ES6에선 spread 연산자를 사용해 apply처럼 배열을 활용해 인자를 넘길 수 있다.

```javascript
function f(a, b) {
  return a + b;
}
const args = [1, 2];
console.log(f(...args)); // 3
```

## bind
bind는 함수를 실행하진 않고, this가 바인딩된 새로운 함수를 리턴한다.

```javascript
function f() {
  console.log(this);
}
const newF = f.bind({ asdf: 1234 });
newF(); // { asdf: 1234 }
```
