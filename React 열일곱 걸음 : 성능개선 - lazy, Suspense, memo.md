# lazy import

: js파일 쪼개기

개발을 마쳤으면 `npm run build` 명령어를 터미널에 입력해
React 코드를 html, css, js 파일로 변환해야한다.

- 그런데! 리액트는 Single Page Application이라 html, js 파일이 하나씩만 생성된다.
- 그럼 뭐다? 한 파일 안에 작성한 모든 내용이 들어있어서 파일 사이즈가 크다.
- 사이즈가 크면? 첫 페이지 로딩하는데 시간이 오래 걸린다~ (당장 필요없는 페이지도 한 번에 불러오니까!)
- 그래서 어떻게 하라는건데??? → js파일을 잘게 쪼개자

### 방법

```jsx
import { lazy } from "react";

const Detail = lazy(() => import("./routes/Detail.js"));
```

# Suspnse

: 로딩 페이지

- 그런데 또! 컴포넌트 로드시 지연시간이 발생할지도?
- 그럴때 보여주고 싶은 내용을 이렇게 써주자.

### 방법

```jsx
<Suspense fallback={<div>로딩중임</div>}>
  <Detail shoes={shoes} />
</Suspense>
```

- 귀찮으면 그냥 <Routes> 전부를 감싸도 괜찮다.

# memo

: 자식은 필요할때만 재렌더링

- 부모 컴포넌트가 재렌더링될 때, 기본적으로 자식도 같이 재렌더링된다.
- 근데 사실 자식은 바뀐게 없고, 자식이 저엉말 다시 렌더링 되어야 할 때에만 그렇게 되게 하고싶으면??
- memo를 사용하면 꼭 필요할 때에만 자식 컴포넌트를 재렌더링 해달라고 할 수 있다.
  - 꼭 필요할 때 예시 : 자식에 전송되는 props가 변경되는 경우

### 방법

```jsx
import { memo } from "react";

// memo를 사용해 자식에 변경점이 있을 때만 재렌더링하기
let MemoExampleChild = memo(function () {
  console.log("메모는 꼭 필요할 때에만 재렌더링됨");
  return <div>자식임ㅇㅇ</div>;
});

// 자식에는 변경점이 없어도 부모와 같이 재렌더링된다.
function Child() {
  console.log("차일드차차차 재렌더링");
  return <div>자식자식차차차</div>;
}
```

- 메모의 단점!!
  - memo로 감싸면 기존 props와 바뀐 props를 비교하는 연산이 추가로 수행된다.
    → props가 크고 복잡하면 이 연산이 부담될 수 있음
  - 꼭 필요한 곳에 사용하자

## useMemo

: 렌더링될 때 실행하고 싶어

- useEffect와 비슷한 용도
- memo로 감싼 컴포넌트에만 쓸 수 있는건 아님!
- 컴포넌트 로드시 1회만 실행하고 싶은 코드가 있다면 여기 담아주자
- useEffect처럼 dependency를 넣어줄 수 있음
  - 특정 state, props가 변할 때에만 실행할 수 있음

### 방법

```jsx
import { useMemo } from "react";

function Child() {
  let result = useMemo(() => {
    return childWillRender();
  }, []);

  console.log("차일드차차차");
  return <div>자식자식차차차</div>;
}
```

# batching

- automatic batching
  ```jsx
  setCount(1);
  setName(2);
  setValue(3); //여기서 1번만 재렌더링됨
  ```
  - state 변경시 재렌더링되기 때문에, 원래라면 3번 되어야 하지만
  - 리액트는 쓸데없는 재렌더링을 방지하기 위해 마지막 한 번만 재렌더링함 → 이게 batching!
- 버전에 따라 일관적이지 않은 batching
  - 그런데! react 17버전까지는 ajax 요청이나 setTimeout 내에 state 변경함수가 있는 경우는 batching이 일어나지 않음
  - 하지만! 18버전 이후부터는 위치가 어디든 재렌더링은 마지막 한 번만 되도록 바꼈다~
- batching이 싫다면?
  - state 변경 함수 실행마다 재렌더링 시키고 싶다면, `flushSync` 를 사용하면 된다.

# **useTransition**

- 렌더링 시간이 오래 걸리는 컴포넌트를 버튼 클릭/타이핑 할 때마다 재렌더링 해야하면?
  → 버튼, 타이핑 반응 속도도 같이 느려짐 → 개선 방법 없을까?
  - 컴포넌트 안의 html 갯수 줄이기. (당연함)
  - 이게 불가능 할 때는 useTransition을 써서 코드 처리 시점을 늦춰주자

### 방법

```jsx
import { useState, useTransition } from "react";

let a = new Array(10000).fill(0);

function App() {
  let [name, setName] = useState("");
  let [isPending, startTransition] = useTransition();

  return (
    <div>
      <input
        onChange={(e) => {
          startTransition(() => {
            setName(e.target.value);
          });
        }}
      />

      {isPending
        ? "로딩중..."
        : a.map(() => {
            return <div>{name}</div>;
          })}
    </div>
  );
}
```

- 10,000개짜리 Array를 만들고, input 내부 값이 바뀔 때 마다 10,000개의 div 값을 input의 value로 변경하는 코드.
- input의 onChange 내부에 state 변경 함수를 넣었기 때문에,
  `startTransition` 적용 전에는
  1. input 내부 값 변경
  2. state 업데이트
  3. div 10,000개에 표시되는 state값에 변경사항 반영
  타이핑시마다 이걸 매번 해줘야 했음
- state 업데이트 코드에 `useTransition` 적용시
  일단 무조건 input을 업데이트 하고
  여유 될 때 한 번에 state 업데이트 및 변경사항 반영을 해주기 때문에
  사용자가 직접 컨트롤하는 input이 빠릿빠릿하게 느껴짐.
- `isPending` 의 사용
  - 얘는 transition 내부의 코드가 기다리고 있는 중이면 true, 외에는 false를 나타낸다.
  - 이 친구를 이용해 로딩 화면 표시 가능!

## useDeferredValue

- `useTransition` 과 비슷한 역할을 하는 친구.

```jsx
import { useState, useTransition, useDeferredValue } from "react";

let a = new Array(10000).fill(0);

function App() {
  let [name, setName] = useState("");
  let nameState = useDeferredValue(name);

  return (
    <div>
      <input
        onChange={(e) => {
          setName(e.target.value);
        }}
      />

      <div> fast {name}</div>

      {a.map(() => {
        return <div>{nameState}</div>;
      })}
    </div>
  );
}
```

- name도 쓸 수 있지만, useDeferredValue 적용해주지 않음.
  → 렌더링 속도가 느리지 않은 컴포넌트에 쓰면 좋을듯.
