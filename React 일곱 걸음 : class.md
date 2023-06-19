으이? 라떼는 말이야?? class,, 라는걸로 컴포넌트를 만들엇다,,,~ 이마리야!

# class

### class로 컴포넌트 만들기

1. 기본 템플릿 만들기

```jsx
class Modal extends React.Component {
	constructor() {
		super()
	}
	render() {
		return (
			// 여기에 html 넣기
		)
	}
}
```

1. state를 쓰고 싶다고??

```jsx
class Modal extends React.Component {
	constructor() {
		super()
		this.state = {
			// 이 안에 state를 object 형식으로 넣는다
			name : 'jeongkite',
			age : 7
		}
	}
	render() {
		return (
			<div>안녕 {this.state.name}</div>
			<button onClikc={()=>{
				this.setState({age : 20});
			}}>버튼</button>
		)
	}
}
```

1. props도 쓰고 싶구나??!

```jsx
class Modal extends React.Component {
	constructor(props) {
		super(props)
	}
	render() {
		return (
			<div>props: {this.props}</div>
		)
	}
}
```