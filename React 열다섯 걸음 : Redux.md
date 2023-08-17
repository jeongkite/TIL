# Redux

### 이게 뭔데?

- props 없이 state를 공유할 수 있게 도와주는 라이브러리
- js파일 하나에 state들을 보관할 수 있음
- props 전송이 필요없어짐 → 컴포넌트가 많아질 수록 편함

## 설정 방법

1. 설치하기

   - 설치시 `pakage.json` 에서 `“react”` , `“react-dom”` 의 버전이 18.1.0 이상인지 확인하기
   - 아래 명령어 터미널에서 실행하기

   ```bash
   npm install @reduxjs/toolkit react-redux
   ```

2. state를 담을 파일 만들기

   - store.js 파일 만들어 아래 내용 입력하기

   ```jsx
   import { configureStore } from "@reduxjs/toolkit";

   export default configureStore({
     reducer: {},
   });
   ```

3. <Provider>로 <App /> 감싸주기

   - `Provider` import 하기
   - 방금 만든 `store` import 하기
   - Provider의 store에 import 해온 store 담기

   ```jsx
   import { Provider } from "react-redux";
   import store from "./store.js";

   const root = ReactDOM.createRoot(document.getElementById("root"));
   root.render(
     <React.StrictMode>
       <Provider store={store}>
         <BrowserRouter>
           <App />
         </BrowserRouter>
       </Provider>
     </React.StrictMode>
   );
   ```

## Redux store에 state 보관하기

1. createSlice()로 state 만들기
2. configureStore()로 등록하기

```jsx
import { configureStore, createSlice } from "@reduxjs/toolkit";

let user = createSlice({
  name: "user",
  initialState: "kim",
});

export default configureStore({
  reducer: {
    user: user.reducer,
  },
});
```

## Redux store에 보관한 state 가져다쓰기

1. `useSelector` import하기
2. useSelector로 불러오기

```jsx
import { useSelector } from "react-redux";

function Cart() {
  let a = useSelector((state) => {
    return state.user;
  });
  console.log(a);

  return 생략;
}
```

## 주의사항

- 간단하게 컴포넌트 몇 없는 서비스를 만들 때는 props를 사용하는게 코드가 더 짧다
  → 항상 redux 사용하는게 이득은 아님.

## State 변경하기

1. store.js 내부에 state 변경 함수 만들기
2. 다른 곳에서 쓰기 좋게 export 하기

```jsx
let user = createSlice({
  name: "user",
  initialState: "kim",
  reducers: {
    changeName(state) {
      return "john " + state;
    },
  },
});

export let { changeName } = user.actions;
```

- reducer : { } 내부에 함수 만들기
  - 함수의 파라미터는 기존 state
  - return 옆에 새로운 값을 넣으면 그걸로 기존 state 변경해줌

1. 원할 때 import해서 쓰기. 근데 `dispatch()` 로 감싸서 사용해야함

```jsx
import { useDispatch, useSelector } from "react-redux";
import { changeName } from "./../store.js";

생략;

let dispatch = useDispatch();

<button
  onClick={() => {
    dispatch(changeName());
  }}
>
  버튼임
</button>;
```

- slice이름.actions : state 변경 함수가 모두 그 자리에 출력된다.

### state 변경 시, 파라미터문법 사용하기

```jsx
let user = createSlice({
  name: "user",
  initialState: { name: "kim", age: 20 },
  reducers: {
    increase(state, action) {
      state.age += action.payload;
    },
  },
});
```

- 변경함수의 두 번째 파라미터를 이용하면 함수 실행시 입력한 파라미터값을 payload 로 불러올 수 있음
- `action.type` 호출 시, state 변경 함수 이름이 나온다.
- array 형식의 state에 새로운 item을 추가하고 싶을 때 : `push` 활용하기
