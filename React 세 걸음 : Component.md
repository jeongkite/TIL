# Component

- html 코드의 중복을 줄이기 위해 반복되는 부분을 묶어서 별명 짓기
- 만드는 방법
    1. function 만들기 : return 밖에 만들어야함
    2. `return()` 안에 html 담기
    3. `<함수명></함수명>` 혹은 `<함수명/>` 쓰기

```jsx
function Modal() {
  return (
    <div className='modal'>
      <h4>제목</h4>
      <p>날짜</p>
      <p>상세 내용</p>
    </div>
  )
}

...
<Modal/>
...
```

### 언제 쓰면 좋은가?

1. 반복적인 html 축약할 때
2. 큰 페이지들
3. 자주 변경되는 것들

### 단점

1. state 가져다 쓸 때 문제가 생김
    
    A함수에 있던 변수는 B함수에서 맘대로 가져다 쓸 수 없음
    → 해결하는 방법 있음