React는 CSS로 스타일 지정하면 다른 js 파일에도 일괄적으로 적용된다.

→ 이걸 극복하기 위해!

- 중복되는 모양을 컴포넌트처럼 만들어두고 편하게 쓰려고! 사용합니다~

- 필요한 css만 <style></style> 태그로 넣어줘서 페이지 로딩시간이 좀 줄어든다

# Styled Components

### 사용 방법

```jsx
import styled from 'styled-components'

let YellowBtn = styled.button`
    background: yellow;
    color: brown;
    padding: 10px;
    border: none;
`
let Textarea = styled.textarea`
    color: red;
`

// 사용하는 방법
<YellowBtn>버튼</YellowBtn>
<Textarea />
```

- styled.`원하는 태그명`
- ```안에 지정할 스타일 넣어주기

  ```
- 컴포넌트처럼 만들어진 내용물 담아주기

## props를 사용할 수 있다구리???

```jsx
let Btn = styled.button`
    background: ${ props => props.bg };
    color: ${ props => props.bg == 'blue' ? 'white' : 'black' };
    padding: 10px;
    border: none;
`

// 사용하는 방법
<Btn bg="blue">버튼</Btn>
```

- 이렇게 props 넣어서 활용해줄 수 있음

## 원래 만들었던거랑 너무 겹친다구리?????

```jsx
let RoundBtn = styled(Btn)`
    border-radius: 30%;
`

// 사용하는 방법
<RoundBtn bg="pink">둥글게둥글게</RoundBtn>
```

- 기존에 만들어놨던 스타일을 상속받을 수 있음.

### 특정 Component에만 적용되는 css파일 만들기

- css 파일명을 `컴포넌트명.module.css` 로 짓기
- 이 css파일 내의 스타일들은 해당 컴포넌트에만 적용된다~

### 단점

- 그냥 컴포넌트인지, 스타일 컴포넌트인지 알아보기가 어려움
- 중복 스타일은 파일별로 다 import 해줘야 할텐데, css 지정해서 className 달아주는거랑 다를게 없음
- 협업시 다른 팀원들간 숙련도 이슈 → 못 알아먹을 수 있음
