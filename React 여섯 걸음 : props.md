# props

- 부모 컴포넌트가 자식 컴포넌트에게 state를 전달해주고 싶을 때 사용한다

### 사용하는 방법

1. `<자식컴포넌트 작명={state 이름}>`
2. props 파라미터 등록 후 `props.작명` 사용

- props 전송은 부모 → 자식만 가능
    - 동위 자식간 전송이나 자식 → 부모 전송 불가능

### 응용 방법

- 파라미터 : 다양한 기능의 함수를 만들 때 사용
    - 실은 props도 파라미터 문법 중 하나임
- 다양한 색상의 모달창이 필요하다고라구라~~??
    - 함수의 parameter와 비슷한 느낌이라고 생각해보자
    
    ```jsx
    <Modal color={'yellow'} />
    
    ~~~
    
    function Modal(props) {
    	return (
    		<div style={{background : props.color}}>나는 모달이다!</div>
    	)
    }
    ```
    
- 함수를 직접 넣을 수도 있다
    
    ```jsx
    <Modal titles={titles} editPost={editPost} />
    
    ~~~
    
    function Modal(props) {
    	return (
    		<button onClick={()=> { props.editPost() }}>글 수정</button>
    	)
    }
    ```
    
    ## 중요. state는 이걸 사용하는 컴포넌트 중 가장 상위 컴포넌트에 만든다.