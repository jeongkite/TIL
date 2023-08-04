React는 기본적으로 싱글페이지 어플리케이션이다.

: `index.html` 하나만 써서 보여줌

→ 이 html 내용물을 계속 갈아끼워주면서 페이지를 보여줘야함.

여기서 팁, 페이지별로 컴포넌트 만들어서 넣어주면 깔끔해진다!

이걸 쉽게 할 수 있도록 도와주는게 `react-router-dom` 과 같은 라이브러리들

# Router

### 설치 방법

[공식 문서 확인하기](https://reactrouter.com/en/main/start/tutorial)

1. 터미널에 `npm install react-router-dom@6` 입력하기
2. index.js 파일에 들어가서 `<App />` 태그 `<BrowserRouter>` 로 감싸주기

```jsx
import { BrowserRouter } from "react-router-dom";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
);
```

## 사용 방법

1. `Routes`, `Route`, `Link` import하기
2. `Routes` 안에 `Route`들 넣어주기
   Route 태그 안에 어떨때 보여줄지 path 속성으로 지정해주기
3. 페이지를 이동하고 싶으면?
   → Link 태그 안에 어디로 이동할지 to 속성으로 지정해주기

```jsx
<Routes>
  <Route path="/" element={<div>여기는 메인</div>} />
  <Route
    path="/detail"
    element={
      <div>
        // 이런저런 태그들. 중요한건 하나로 묶어줘야한다. 귀찮으면 컴포넌트로
        만들어서 깔끔하게 넣자.
        <Link to="/">홈</Link>
      </div>
    }
  />
  <Route path="/about" element={<div>여기는 어바웃</div>} />
</Routes>
```

## 유용한 기능

### 404 페이지

잘못된 접근 페이지 만들기

path값을 `*` 로 지정하면 그 외의 모든 페이지에 대응한다.

```jsx
<Route path="*" element={<div>잘못된 접근입니다.</div>} />
```

### Nested Routes

중복되는 path값이 신경쓰일 때는, Route도 부모, 자식 관계로 사용할 수 있다.

```jsx
<Route path="/about" element={<AbuoutPage />}>
  <Route path="member" element={<Member />} />
  <Route path="location" element={<Location />} />
</Route>
```

→ 이러면 member는 `/about/member` , location은 `/about/location` 이런식으로 path가 지정된다.

- 중요한 점!!
  nested된 자식 Route의 element도 함께 보여주게 된다.
  → `/about/member` 접속 시, `<AboutPage />` 랑 `<Member />` 를 같이 보여준다는 얘기.
  - 추가로 AboutPage의 어느 부분에 Member를 보여줄지 설정해야 한다.
    → 이걸 해주는게? : `Outlet`

# Outlet

```jsx
function AbuoutPage() {
  return (
    <div>
      <h4>회사 정보 입니다</h4>
      <Outlet></Outlet>
    </div>
  );
}
```

- Outlet 태그 위치에 구멍을 뚫어준다는 느낌.
  이 구멍에 자식 element가 쏘옥 들어간다
