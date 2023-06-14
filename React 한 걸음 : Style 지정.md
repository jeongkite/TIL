# class 지정

- class 명령어가 겹친다
- JSX에서는 className이라고 쓰기로 했다

```jsx
<div className="black-nav">네에비게이셔어언</div>
```

## id 지정

- id는 기본 html이랑 동일하다

```jsx
<div id="black-nav">네에비게이셔어언</div>
```

# 인라인 Style 지정

- style 지정값은 object 자료형식으로 집어넣어야한다
- `-`는 빼기로 인식되기 때문에 fontSize라고 카멜케이스 사용해야한다

```jsx
<div style={ {color: "red", fontSize: "16px"} }>
	유후~ 앤 나하이~~
</div>
```
