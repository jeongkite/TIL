# 변수

```jsx
let post = '강남 우동 맛집';
<div>{ post }</div>
```

- const, let, var로 선언 가능
- var
    - 업데이트, 재선언 가능
    - 전역, 함수 범위
        - 함수 내에서 선언시 해당 함수 내에서만 사용 가능
        - 함수  내에 있지 않으면 전역 범위를 가짐
- let
    - 블록 범위
    - 업데이트 가능
    - 재선언 불가
- const
    - 블록 범위
    - 재선언 및 업데이트 불가

```jsx
const greeting = {
	message: "Say hi!",
	times: 4
}

// 불가능한 변경
greeting = {
	words: "Hello",
	number: "five"
}
// 가능한 변경
greeting.message = "Say Hello~";
```

# State

- 데이터를 잠시 담아두는 그릇임은 변수와 동일
- 다른점은, 값을 변경했을 때 html에도 자동으로 반영된다.
→ 변경된 state를 쓰던 html은 자동으로 재렌더링된다!
    - 변동시 자동으로  html에 반영되게 만들고 싶을 때 state를 사용한다.
        - ex. 제목, 날짜, class명 등…

```jsx
let [post, updatePost] = useState('봄웜 팔레트 추천');
```

- post : 해당 데이터를 담아두는 이름
- updatePost : 해당 값 업데이트를 위한 함수명

## State 값 변경하기

```jsx
updatePost('변경할 제목');
```

- 기존 state와 비교(===)하여 값이 동일하면 변경해주지 않는다

### array, object state 변경

- 원본값을 바로 변경하는게 아닌, 업데이트할 값의 변수를 따로 만드는게 좋다.
- js의 경우, array와 object는 레퍼런스 타입이다
    - 그래서 posts에는 실제 array의 위치만 저장되어 있음
    - array의 값을 바꿔도 posts에 저장된 array의 위치값은 변경되지 않았음
    - 결론적으로 두 값이 동일하다고 생각해 업데이트 해주지 않음!!
- 해결 방법.
    - array 대괄호를 벗겼다가 다시 씌우기
    - `newPosts = [...posts];`
    - 이렇게 하면 아예 새로운 array와 그에 해당하는 위치값이 생기기 때문에 다르다고 판단해 업데이트 해준다.

```jsx
let [posts, updatePosts] = useState(['글제목1', '글제목2', '글제목3'];
let newPosts = [...posts];
newPosts[0] = '봄웜 찰떡립 Top5';
updatePosts(newPosts);
```
