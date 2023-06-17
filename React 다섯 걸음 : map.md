# Map

- 이 함수가 하는 일
    - array 자료 개수만큼 함수 안의 코드를 실행해줌
    - 함수의 파라미터는 array 안에 있던 자료임
    - return에 뭐 적으면 array로 담아줌

```jsx
[1, 2, 3].map(function(a, i) {
	return (
		<div key={i}>{ a }</div>
	)
})

// 1, 2, 3이 div 안에 담겨서 표시
```

- 이때 map 반복문으로 반복생성한 html엔 `key={i}` 이런 속성을 추가해야만 React가 각각을 구분할 수 있어 경고가 사라짐

### 그냥 일반 반복문 쓰는 방법

```jsx
function App (){
  
  var 어레이 = [];
  for (var i = 0; i < 3; i++) {
    어레이.push(<div>안녕</div>)
  }
  return (
    <div>
      { 어레이 }
    </div>
  )
}
```

- for 문법은 jsx 내에서 쓸 수 없어 위처럼 바깥에 사용해야한다