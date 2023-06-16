# 동적인 UI?

### 종류

- 모달
- 탭
- 툴팁
- 햄버거
- 경고문

## 만드는 방법

1. html, css로 미리 디자인 완성
2. UI의 현재 상태를 state로 저장
3. state에 따라 UI가 어떻게 보일지 조건문 등으로 작성
    1. 이때 if문이 아닌 삼항연산자 이용 필요

```jsx
let [modal, setModal] = useState(false);
{ modal ? <Modal/> : null }
```

- 그냥 javascript였으면 버튼 눌렀을 때 모달창 html을 직접 건드림
- 리액트에선 버튼 누르면 모달창 스위치(state)만 건드림