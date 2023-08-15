### 방법이 세 개지요~

1. XMLHttpRequest : 옛날 js 문법
2. fetch() : 요즘 js 문법
3. axios : 외부 라이브러리

→ 오늘은 axios 사용

# axios

### 설치

```jsx
npm install axios
```

```jsx
import axios from "axios";
```

## GET 요청 보내기

```jsx
<Button
  onClick={() => {
    axios
      .get("https://codingapple1.github.io/shop/data2.json")
      .then((result) => {
        let newItems = items.concat(result.data);
        setItems(newItems);
      })
      .catch(() => {
        console.log("🚨 네트워크 통신 실패!");
      });
  }}
  variant="outline-secondary"
  className="my-5"
>
  더 보기
</Button>
```

- `result.data` 는 사실 기본적으로 json 형식이지만, axios가 array로 자동으로 바꿔준다

## POST

```jsx
axios
  .post("URL", { data: "kite" })
  .then((result) => {
    let newItems = items.concat(result.data);
    setItems(newItems);
  })
  .catch(() => {
    console.log("🚨 네트워크 통신 실패!");
  });
```

## 동시에 여러개 요청

```jsx
Promise.all([ axios.get('/url1'), axios.get('/url2') ])
.then(() => {
		// 두 요청이 동시에 성공했을 때
)}
```

# fetch 사용시

## 주의점!!

axios는 json 형식이던 data를 자동으로 변경해주지만, 기본 함수 사용시 따로 변경해줘야 한다.

### 방법

```jsx
fetch("url")
  .then((result) => result.json())
  .then((result) => {
    console.log(result.data);
  });
```

- JSON → array / object 변환 과정 필요
