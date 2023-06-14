# RxSwift로 데이터를 보내는 방법

## 1. 정석적인 방법

```swift
func getData(_ url: String) -> Observable<String?> {
		return Observable.create() { emitter in
        emitter.onNext("Hello World")
        emitter.onCompleted()
        return Disposables.create()
    }
}
```

```swift
// 결과
// Optional("Hello World")
```

# Suger API

## 2. just

- 하나만 보낼건데 코드가 너무 길다면?
- 데이터를 하나만 보내는 경우 더 간단하게 쓰는 방법

```swift
func getData(_ url: String) -> Observable<String?> {
		return Observable.just("Hello World")
}
```

```swift
// 결과
// Optional("Hello World")
```

- 굳이굳이 여러개를 보내고 싶다면?

```swift
func getData(_ url: String) -> Observable<[String?]> {
		return Observable.just(["Hello", "World"])
}
```

```swift
// 결과
// [Optional("Hello"), Optional("World")]
```

## 3. from

- 데이터 여러개를 간단하게 보내고 싶다면?

```swift
func getData(_ url: String) -> Observable<String?> {
		return Observable.from(["Hello", "World"])
}
```

```swift
// 결과
// Optional("Hello")
// Optional("World")
```

## 4. subscribe도 더 쉽게

- 정석적인 방법

```swift
_ = getData(MEMBER_LIST_URL)
    .subscribe{ event in
    switch event {
    case .next(let t):
        print(t)
        break
    case .error(let err):
        break
    case .completed:
        break
    }
}
```

- 클로저 이용~

```swift
_ = getData(MEMBER_LIST_URL)
		.subscribe(onNext: { print($0) },
        onError: { err in print(err) },
	      onCompleted: { print("Complete") } )
```

이렇게 간단하게 데이터 확인 가능!

## 5. observeOn

- 스레드 변경을 더 쉽게~

```swift
downloadJson(MEMBER_LIST_URL)
    .observeOn(MainScheduler.instance)  // suger api
    .subscribe(onNext: { json in
        self.editView.text = json
        self.setVisibleWithAnimation(self.activityIndicator, false)
    })
```

## 6. 다양한 operator들~

```swift
downloadJson(MEMBER_LIST_URL)
    .map { json in json?.count ?? 0 }
    .filter { count in count > 0 }
    .map { "\($0)" }
    .observeOn(MainScheduler.instance)  // suger : operator
    .subscribe(onNext: { json in
        self.editView.text = json
        self.setVisibleWithAnimation(self.activityIndicator, false)
    })
```

### 더 많은 내용은…

[공식문서 참고하기](https://reactivex.io/documentation/operators.html)

### 출처

[https://www.youtube.com/watch?v=iHKBNYMWd5I&list=PL03rJBlpwTaBrhux_C8RmtWDI_kZSLvdQ](https://www.youtube.com/watch?v=iHKBNYMWd5I&list=PL03rJBlpwTaBrhux_C8RmtWDI_kZSLvdQ)
