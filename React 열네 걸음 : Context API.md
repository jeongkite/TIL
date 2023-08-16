# Context API

### 이게 뭐임?

- 컴포넌트끼리 중첩되어 있을 때 props로 데이터 전달하는게 귀찮음
- 이럴 때 쓸 수 있는 방법 중 Redux 혹은 Context API가 있음

## 사용 방법

### context 만들기

- context란? : state 보관함

```jsx
import { useState, createContext } from "react";

export let Context1 = createContext();
```

### Context로 원하는 곳 감싸고 공유하려는 state를 value에 담기

```jsx
function App() {
  let [stock, setStock] = useState([1, 7, 4]);
  let [items, setItems] = useState([1, 7, 4]);

  return (
    <Context1.Provider value={{ stock, items }}>
      <Detail shoes={shoes} />
    </Context1.Provider>
  );
}
```

### Context에 담아 전달했던 State 꺼내 사용하기

1. 만들어둔 Context import 해오기
2. useContext로 state 분리하기

```jsx
import { useState, useContext } from "react";
import { Context1 } from "../App.js";

function Detail() {
  let states = useContext(Context1);
  return <div>{states}</div>;
}
```

- Detail 내의 모든 자식 컴포넌트도 useContext를 통해 자유롭게 Context1 내의 state을 사용할 수 있다.
- 중첩해 사용한 컴포넌트가 많을 때 편리한 문법이다.

## Context API의 단점

- state 변경시 쓸데없는 컴포넌트까지 전부 재렌더링 된다
- useContext()를 사용하는 컴포넌트를 추후 다른 파일에서 재사용할 때 Context를 import 하는게 귀찮아질 수 있다.
- 그래서 이것 보다는 redex 등 외부 라이브러리를 많이 쓴다.
