## 설치

```bash
npm install @tanstack/react-query
```

## 사용

- index.js
    - 1, 2, 3번 순서대로 하기

```jsx
import { QueryClient, QueryClientProvider } from "react-query"  //1번
const queryClient = new QueryClient()   //2번

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <QueryClientProvider client={queryClient}>    //3번
    <Provider store={store}>
      <BrowserRouter>
        <App />
      </BrowserRouter>
    </Provider>
  </QueryClientProvider>
);
```

- ajax 요청시 사용 방법

```jsx
function App() {
	let result = useQuery(['query'], () => 
		axios.get('https://codingapple1.github.io/userdata.json')
			.then((a) => { return a.data })
	)

	return (
		<div>
			{ result.isLoading && '로딩중' }
			{ result.error && '에러남' }
			{ result.data && result.data.name }
		</div>
}
```

- 장점
    1. ajax 요청 성공/실패/로딩중 상태 파악 쉬움
    2. 시간 좀 지나면 알아서 재요청 해줌
        - 재요청을 언제 하는데??
            - 페이지 체류후 일정 시간이 지나거나
            - 다른 창으로 갔다가 다시 페이지로 돌아오거나
            - 다시 메인페이지로 돌아가거나
        - 물론 끄거나, 재요청하는 간격 조절하는 방법도 있음
    3. 실패하면 알아서 재시도 해줌
    4. 캐싱 기능이 있어서 state 공유 필요 없음
        - 다른 컴포넌트에 똑같은 api 요청하는 코드 또 적어도 
        react-query는 똑똑해서 요청이 여러개 있으면 한 개만 날려주고
        - 캐싱 기능이 있어 이미 같은 요청을 했었으면 그 결과를 우선 가져다가 쓴다.