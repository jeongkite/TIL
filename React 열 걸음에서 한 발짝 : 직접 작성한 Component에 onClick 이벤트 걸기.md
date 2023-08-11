## 문제 상황

코딩애플 강의를 듣다가 내가 만든 컴포넌트를 클릭했을 때 페이지 이동을 해주고 싶었다.

그런데!

### 내가 맨 처음 생각한 방법

```jsx
<Card
  onClick={() => {
    navigate("/detail/" + i);
  }}
  item={item}
/>
```

- Card가 내가 만든 컴포넌트
- 이렇게 컴포넌트 내부에 직접 넣으려고 했는데 작동하지 않았다!

### 가능한 방법

```jsx
function Card(props) {
  let navigate = useNavigate();

  return (
    // 여기다 넣어주면 동작함!!
    <div
      onClick={() => {
        navigate("/detail/" + props.item.id);
      }}
      className="col-md-4"
    >
      <img src={"/present" + (props.item.id + 1) + ".jpeg"} width="80%" />
      <h4>{props.item.title}</h4>
      <p>{props.item.content}</p>
      <p>{props.item.price}</p>
    </div>
  );
}
```

- 컴포넌트 구현부에 넣어주면 잘 동작했다.

### 이유

- 컴포넌트는 여러 엘리먼트(div, a와 같은 일반 태그)를 포함한 인스턴스를 반환하는데
  이게 DOM 엘리먼트와 일치하지 않는다.
  → 그래서 onClick과 같은 DOM 이벤트를 바인딩할 수 없다.
- 맨 처음 생각한 방법에서 onClick은 props로 컴포넌트에 전달된다.

### → 만약 props로 onClick 이벤트를 전달하고 싶다면?

```jsx
<Card customClickEvent={this.chosenResource.bind(this)} />
```

```jsx
function Card(props) {
  let navigate = useNavigate();

  return (
    // 여기서 다시 바인딩 해줘야함!!
    <div onClick={this.props.customClickEvent} className="col-md-4">
      <img src={"/present" + (props.item.id + 1) + ".jpeg"} width="80%" />
      <h4>{props.item.title}</h4>
      <p>{props.item.content}</p>
      <p>{props.item.price}</p>
    </div>
  );
}
```

[참고 링크](https://stackoverflow.com/questions/45727171/onclick-does-not-work-for-custom-component)
