# [Combine](https://developer.apple.com/documentation/combine)

## 어떻게 이루어질까?

![컴바인의 패턴](https://user-images.githubusercontent.com/75439868/214284004-faf69401-31da-4f5e-80e7-f2fbcbfdcbd0.png)

- 서브스크라이버는 퍼블리셔에게 붙는다
- 퍼블리셔는 서브스크립션을 보낸다
- 서브스크라이버는 N개의 값을 요청한다
- 퍼블리셔는 N개이거나 그보다 적은 값을 보낸다
- 퍼블리셔는 컴플리션을 보낸다

## Combine의 주요 컴포넌트

- Publisher : 생산자, 크리에이터, 배출자, 배설자
- Subscriber : 소비자, 구독자, 받는 사람
- Operator : 변경시키는 사람, 마법사, 가공하는 사람

![Frame 79](https://user-images.githubusercontent.com/75439868/214284104-ee7cca3a-c2ce-4583-b3c8-d890080b49f1.png)

- 퍼블리셔가 배출한 데이터를 서브스크라이버가 받는 형태
- 오퍼레이터는 이 데이터를 서브스크라이버에게 그대로 줄 수도, 변경해서 줄 수도 있다. (중간자 역할)

## 그래서 그게 뭔데?

### [Publisher](https://developer.apple.com/documentation/combine/publisher)

```swift
protocol Publisher {
		associatedtype Output
		associatedtype Failure: Error
}
```

 → 구체적인 output 및 실패 타입(Failure) 정의 필요

- 데이터를 배출
    - Subscriber가 요청한 데이터 제공
- 빌트인 Publisher
    - `Just` : 값을 다룬다
    - `Future` : Function을 다룬다
- iOS에서 기본으로 제공하는 Publisher
    - NotificationCenter
    - Timer
    - URLSession.dataTask

### [Subscriber](https://developer.apple.com/documentation/combine/subscriber)

```swift
protocol Subscriber {
		associatedtype Input
		associatedtype Failure: Error
}
```

→ Input 및 실패 타입(Failure) 정의 필요

- Publisher에게 데이터를 요청
    - Publisher 구독 후, 필요한 데이터를 개수와 함께 요청
    - 파이프라인 취소 가능
- 빌트인 Subscriber
    - `assign` : Publisher가 제공한 데이터를 특정 객체의 키패스에 할당
    - `sink` : Publisher가 제공한 데이터를 받을 수 있는 closure 제공

### [Subscription](https://developer.apple.com/documentation/combine/subscription)

- Publisher와 Subscriber가 연결되어 있음을 나타내는 것
    - Publisher가 발행한 구독권
    - 이게 있으면 데이터를 받을 수 있음
    - 이 구독권이 사라지면 구독 관계도 사라짐
- `Cancellable` protocol을 따르고 있다
    - 그러므로 Subscription을 통해 구독을 취소할 수 있다

### Subject (Publisher)

: 특별한 형태의 퍼블리셔

- `send(_:)` 메소드를 이용해 이벤트 값을 주입시킬 수 있는 퍼블리셔
- 기존의 비동기 처리 방식에서 Combine으로 전환할 때 유용하다
- 빌트인 타입
    - `PassthroughSubject`
        - Subscriber가 데이터를 요청하면
        - 요청 이후부터 받은 데이터를 전달
        - 전달한 데이터를 가지고 있지 않음
    - `CurrentValueSubject`
        - Subscriber가 데이터를 요청하면
        - 최근에 가지고 있던 값을 전달 + 요청 이후부터 받은 데이터 전달
        - 최근에 전달한 데이터를 가지고 있음

### @Published (Publisher)

: 얘도 결국엔 퍼블리셔

- `@Published` 로 선언된 프로퍼티를 퍼블리셔로 만들어준다
- 클래스에 한해 사용 (구조체에서는 사용할 수 없다)
- `$` 를 이용해 퍼블리셔에 접근할 수 있다

```swift
class Weather {
		@Published var temperature: Double
		init(temperature: Double) {
				self.temperature = temperature
		}
}

let weather: Weather = Weather(temperature: 20)
let subscription = weather.$temperature.sink {
		print("Temperature now: \($0)")
}
weather.temperature = 25

// Temperature now: 20.0
// Temperature now: 25.0
```

### Operator

- Publisher에게 받은 데이터를 가공해서 Subscriber에게 제공
- 기본적으로 Input, Output, Failure type을 받는다
    - 모두 동일한건 아니다.
    - 타입이 다를 수 있음
- 빌트인 Operator가 많다
    - map, flatMap, contains, merge, drop, filter, reduce, collect, combineLatest…

### Scheduler

**정의**

- Subscriber의 sink 등에서 정의한 closure를 언제 어떻게 실행할지 정해준다
- Operator의 파라미터로 Scheduler를 받는 경우도 있다
    - 작업에 따라, 백그라운드 혹은 메인스레드에서 실행될 수 있도록 도와준다
- Scheduler가 스레드 자체는 아니다

**두 가지 Scheduler 메소드**

- `subscribe(on:)` : publisher가 어느 스레드에서 작업을 수행할지 결정
    - 무거운 작업은 메인이 아닌 다른 스레드에서 작업할 수 있게 도와준다
    - ex.
        - 백그라운드 계산이 많이 필요한 것
        - 파일을 다운로드 해야하는 경우
- `receive(on:)` : operator, subscriber가 어느 스레드에서 작업을 수행할지 결정
    - UI 업데이트가 필요한 데이터를 메인스레드에서 받을 수 있게 도와준다
    - ex.
        - 서버에서 가져온 데이터로 UI를 업데이트 할 때

**Pattern**

- 일반적인 패턴

```swift
let jsonPublisher = MyJasonLoaderPublisher()

jsonPublisher
		.subscribe(on: backgroundQueue)
		.receive(on: RunLoop.main)
		.sink { value in 
				label.text = value
		}
```

→ 위 코드를 도식화 하면 아래 이미지처럼 나타낼 수 있다.

![Frame 80](https://user-images.githubusercontent.com/75439868/214284148-db3aeb7b-2666-4af0-b21a-0c31b1968130.png)


- **UI 업데이트 시,**
    - ⛔️ 할 수는 있지만 컴파인을 사용하는데 굳이 싶은 방법
    
    ```swift
    publisher.sink {
    		DispatchQueue.main.async {
    				// update UI
    		}
    }
    ```
    
    - ✅ 컴바인을 사용할 때는 이렇게 하자!
    
    ```swift
    publisher.receive(on: DispatchQueue.main).sink {
    		// update UI
    }
    ```
