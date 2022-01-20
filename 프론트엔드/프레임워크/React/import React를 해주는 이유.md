React 17 이하에선 JSX를 사용하기 위해 항상 아래 코드가 있어야 한다.

```typescript
import React from "react";
```

그런데 이걸 왜해주는걸까? 그 이유는 JSX 코드가 바벨을 통해 React.createElement로 변환되기 때문이다.

```tsx
import React from "react";

function Component() {
  return <h1>Hello, World</h1>
}
```

```typescript
import React from "react";

function Component() {
  return React.createElement("h1", null, "Hello, World");
}
```

변환 결과물에 import React가 없었다면 어떻게 될까? createElement를 찾을 수 없다는 에러가 날 것이다.

그럼 React 17부터는 왜 import React를 해주지 않아도 되는걸까?

그 이유는 React.createElement에서 벗어난 새로운 Transform 방식이 적용되었기 때문이다. React 17부터 JSX는 아래처럼 변경된다.

```typescript
import { jsx as _jsx } from "react/jsx-runtime";

function Component() {
  return _jsx("h1", { children: "Hello world" });
}
```
