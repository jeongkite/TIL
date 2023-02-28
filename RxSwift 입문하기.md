# Rx란?

- 기본적으로 비동기적으로 움직이는 애플의 API들과 수시로 상태가 변하는 환경에서 보다 직관적이고 효율적으로 코드를 작성할 수 있도록 도와준다

## Observable

```swift
Observable
	.combineLatest(
		userName.rx.text,
		email.rx.text
	) { $0 + " (" + $1 + ")" }
.map { "Welcome, \($0)" }
.bind(to: successLabel.rx.text)
.disposed(by: disposeBag)
```

- RxCocoa가 제공하는 bind를 통해 UI를 업데이트

## retry

```swift
doSomethingIncredible()
	.retry(5)
```

- 재시도(재시도 횟수)

## Delegate

- 기존 Delegate 패턴의 경우
    - 명시적이지 않음
        - 전체 코드를 확인해야함.
        - 어떤 스크롤뷰에 명령어가 적용되는지, 델리게이트를 선언했는지 등을 직관적으로 확인하기 힘듦.

```swift
public func scrollViewDidScroll(scrollView: UIScrollView) { [weak self]
	self?.leftPositionConstraint.constant = scrollView.contentOffset.x
}
```

- Rx를 사용하면

```swift
self.resultsTableView.rx.contentOffset
	.map { $0.x }
	.bind(to: self.leftPositionConstraint.rx.constant)
```

## 그 외 비동기 API들

- Notification Center
- Delegate Pattern
- Grand Central Dispatch(GCD)
- Closures
- Combine

## Rx의 이점

- Composable : 조합 가능
- Reusable : 재사용 가능
- Declarative :  선언형 - 정의를 변경하는게 아닌, 오퍼레이터를 통해 데이터만 변경
- Understandable and concise
- Stable
- Less stateful
- Without leaks
