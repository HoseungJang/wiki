자바스크립트의 this는 자기 참조 변수이다.

함수의 this는 기본적으로 window다. (Node.js에서는 globalThis)

```javascript
function f() {
  return this;
}

console.log(f() === global); // true
```

이 때, 함수가 어떻게 호출되었는지에 따라 this는 달라진다. 아래에 this가 달라지는 경우를 정리해보겠다.

## 객체의 메소드
객체의 메소드는 자신이 속한 객체를 this로 갖는다.

```javascript
const o = {
  n: 1234,
  f() {
    return this.n;
  }
}
console.log(o.f()); // 1234
```

여기에 프로토타입 체인을 합쳐보면 되게 신기한 동작을 하는데,

```javascript
const o = {
  f() {
    return this.a + this.b;
  },
};
const p = Object.create(o);
p.a = 1000;
p.b = 1000;
console.log(p.f()); // 2000
```

프로퍼티 a, b가 존재하지 않는 객체 o를 만들고, o를 프로토타입으로 삼는 객체 p를 만들었다.

그리고 p에 프로퍼티 a, b를 각각 정의해준 후, f를 실행해보면 놀랍게도 2000을 출력한다.

f가 p의 메소드로써 실행된 시점부터, f의 this는 p에 바인딩되기 때문에 이런 동작이 가능한 것이다.

다른 예제를 들어보면, 외부 함수를 객체의 메소드로써 할당하고 실행하는 경우에도 그 함수의 this는 해당 객체로 새롭게 바인딩된다.

```javascript
const o = {
  a: 1,
};

function f() {
  console.log(this.a);
}

f(); // undefined

o.f = f;
o.f(); // 1
```

## 생성자
함수에 new 키워드를 사용해 객체를 생성하면, 함수의 this는 새롭게 생성된 그 객체에 묶인다.

```javascript
function f() {
  this.a = 1;
}
const o = new f();
console.log(o.a); // 1
```

## this를 강제로 주입하는 경우
Function.prototype.call 등의 빌트인 메소드를 사용하면, 함수가 어떻게 호출되었는지랑은 상관없이 개발자 마음대로 this를 설정할 수 있다.

```javascript
const o = {
  a: 1234,
  f() {
    console.log(this.a);
  },
};
o.f.call({ a: 4321 }); // 4321
```

## array function
array function은 호출 방법에 따라 this 바인딩이 결정되는게 아니라, 자신이 속한 lexical environment가 어디냐에 따라 결정된다.

```javascript
function f() {
  return () => {
    return this;
  }
}
const getThis = f.call({ asdf: 1234 });
console.log(getThis()); // { asdf: 1234 }
```

위 arrow function의 this는 자신이 속한 lexical environment인 f의 this에 바인딩된다.
