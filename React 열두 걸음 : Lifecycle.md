# 컴포넌트의 Lifecycle

- mount : 페이지에 장착되기도 하고
- update : 가끔 업데이트 되기도 하고 (리렌더링 Re-render)
- unmount : 필요 없으면 제거되고

→ 중간중간 간섭(코드 실행) 가능 : 갈고리(hook) 달기

## 컴포넌트 생명주기에 따라 코드 실행하기

### class로 컴포넌트 정의 시,

```jsx
class MyComponent extends React.Component {
  componentDidMount() {
    // mount 됐을 때 실행할 코드
  }
  componentDidUpdate() {
    // update 됐을 때 실행할 코드
  }
  componentDidUnmount() {
    // unmount 됐을 때 실행할 코드
  }
}
```

### function으로 컴포넌트 정의 시,

```jsx
function MyComponent(props) {
  useEffect(() => {
    // mount, update(re-render) 됐을 때 실행할 코드
  });
}
```

- 개발 환경에서는 useEffect 내의 코드가 두 번 실행되는데, 발행 후 실제 서비스에서는 한 번 동작할 것
  → 막는 방법 : index.js 파일에서 `<React.StrictMode>` 제거하기
- 그런데! useEffect 바깥, 그냥 function 정의 내부에 적어도 똑같이 컴포넌트 mount, update시에 실행된다.
  - 왜냐면 컴포넌트 mount, update시 function 내부의 코드를 다시 읽고 지나가기 때문.
  - 그럼 useEffect는 뭐가 다를까??
    - **useEffect 내부의 코드는 html 렌더링 이후에 동작한다.**
      → 그래서 실행 시간이 오래 걸리는데, UI 로드시에는 필요하지 않은 코드가 있다면 useEffect 내부에 적어주는게 좋다.
      useEffect를 통해 코드 실행 시점 조절 가능
- 결과적으로 html 렌더링 속도를 향상시킬 수 있음
  - side effect : 함수의 핵심 기능 외에 쓸데없는 기능들
    → 이름이 useEffect인 이유
    - 컴포넌트의 핵심 기능은 html 렌더링
    - 그 외에 오래 걸리는 반복연산, 서버에서 데이터 가져오는 작업, 타이머 등을 useEffect에서 처리하는게 좋다

# Dependency

### 특정 state가 변할 때 코드를 실행하고 싶다면

```jsx
useEffect(() => {
  setTimeout(() => setIsBoxHidden(true), 2000);
}, [count]);
```

- count가 변할 때마다 useEffect 내부 코드 실행
- 맨 처음 mount 될 때 실행되는건 동일하다

### 근데 dependency를 비워둔다면?

```jsx
useEffect(() => {
  setTimeout(() => setIsBoxHidden(true), 2000);
}, []);
```

- 맨 처음 mount시에만 딱 한 번 실행되고 update되는 경우는 실행하지 않는다

# useEffect의 return

```jsx
useEffect(() => {
  // return 실행 다음에 실행된다
  return () => {
    // useEffect 동작 전에 실행하고 싶은 코드 작성하기
  };
});
```

- clean up function
  useEffect가 실행되기 전에 return 내부의 코드가 실행된다.
  - 컴포넌트 mount시에는 실행되지 않고, unmount 시에만 clean up function(return) 내부의 코드가 1회 실행된다.
  - 보통 언제 사용하냐구?
    - 타이머 제거, socket 연결 요청 제거, ajax 요청 중단 등..
- 한 컴포넌트 내에 실행 조건이 다른 useEffect 중복 사용 가능
