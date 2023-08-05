머라구?? 페이지 이동을 더 편하고 쉽게 하고 싶다구????

# useNavigate

### Link의 경우

```jsx
<Link to="/">홈</Link>
<Link to="/detail">상세페이지</Link>
```

: `a` 태그를 삽입해준다

→ 못생겼다

### 사용 방법

- `useState` 처럼 이걸 불러오면 유용한 함수가 남는다.

1. 변수에 담아서 `onClick` 안에 담아주자
2. 매개변수 자리에 이동할 path값을 넣어준다.

```jsx
function App(){
  let navigate = useNavigate()

  return (
    (생략)
    <button onClick={()=>{ navigate('/detail') }}>이동버튼</button>
  )
}
```

- 추가로 사용하면 유용한 기능들
  - 숫자를 넣어주면 해당 위치로 이동할 수 있다.
  - 뒤로 가기 : `navigate(-1)`
  - 앞으로 가기 : `navigate(2)`

# URL parameter

Router 사용 시, 상세페이지 등 정보만 다르고 모양은 똑같은 페이지를 쉽고 편하게 여러개 만들고 싶다면??

→ Route 태그의 path값에 parameter를 지정해주자

### 사용 방법

```jsx
// 파라미터 지정
<Route path="/detail/:itemId" element={<DetailPage items={items} />} />;

// 파라미터 받아오기
import { useParams } from "react-router-dom";

function DetailPage(props) {
  let { itemId } = useParams();

  <h4 className="pt-5">{props.items[itemId].title}</h4>;
}
```

- 이때, 지정해준 값이랑 동일한 이름으로 받아와야 한다.

### 추가로 유용한 기능

- `find` 로 object array에서 원하는 object 꺼내오기
  ```jsx
  let { itemId } = useParams();
  let item = props.items.find((item) => item.id == itemId);
  ```
  - find 함수 : 삽입한 조건에 맞는 아이템을 찾고 가장 처음 발견한 친구만 넘겨준다
  - 조건 삽입 방법
    - 정직하게 함수로 넣기
      ```jsx
      find(function (item) {
        item.id == itemId;
      });
      ```
    - 화살표 함수로 넣기
      ```jsx
      find((item) => {
        return item.id == itemId;
      });
      ```
    - 추가로 더 축약해보면?
      → 한 줄짜리 코드는 중괄호 및 return 생략 가능
      ```jsx
      find((item) => item.id == itemId);
      ```
