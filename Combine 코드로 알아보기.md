# 1. Publisher & Subscriber

```swift
// 데이터를 한 번 전송하고 끝
let just = Just(1000) 
let subscription1 = just.sink { value in
    print("Just, Received Value: \(value)")
}

let arrayPublisher = [1, 3, 5, 7, 9].publisher
let subscription2 = arrayPublisher.sink { value in
    print("arrayPublisher, Received Value: \(value)")
}

class MyClass {
    var property: Int = 0 {
        didSet {
            print("Did set property to \(property)")
        }
    }
}
let myClassObject = MyClass()
let subscription3 = arrayPublisher.assign(to: \.property, on: myClassObject)
//myClassObject.property = 3
print("Final value \(myClassObject.property)")

```

```
💻 # 결과

 Just, Received Value: 1000
 arrayPublisher, Received Value: 1
 arrayPublisher, Received Value: 3
 arrayPublisher, Received Value: 5
 arrayPublisher, Received Value: 7
 arrayPublisher, Received Value: 9
 Did set property to 1
 Did set property to 3
 Did set property to 5
 Did set property to 7
 Did set property to 9
 Final value 9
```

# 2. Subject

```swift
// PassthroughSubject
let relay = PassthroughSubject<String, Never>()
let subscription1 = relay.sink { value in
    print("subscription1 received value: \(value)")
}

relay.send("Hello")
relay.send("World!")

// CurrentValueSubject
let variable = CurrentValueSubject<String, Never>("")
variable.send("Initial text")
let subscription2 = variable.sink { value in
    print("subscription2 received value: \(value)")
}

variable.send("More text")
variable.value

let publisher = ["My", "name", "is", "jeongkite!"].publisher
publisher.subscribe(relay)
```

```
💻 # 결과

subscription1 received value: Hello
subscription1 received value: World!
subscription2 received value: Initial text
subscription2 received value: More text
subscription1 received value: My
subscription1 received value: name
subscription1 received value: is
subscription1 received value: jeongkite!
```

# 3. Subscription

```swift
let subscription = subject
    .print("[Debug] ")    // 흐름을 보여준다
    .sink { value in
    print("Subscriber received value: \(value)")
}

subject.send("Hello")
subject.send("Hello again!")
subject.send("Hello for the last time!")
//subject.send(completion: .finished)     // 데이터 전송 끝!
subscription.cancel()                   // subscription에서 관계를 해제할 수도 있음.
subject.send("Hello??")                 // 출력되지 않음.
```

```
💻 # 결과

[Debug] : receive subscription: (PassthroughSubject)
[Debug] : request unlimited
[Debug] : receive value: (Hello)
Subscriber received value: Hello
[Debug] : receive value: (Hello again!)
Subscriber received value: Hello again!
[Debug] : receive value: (Hello for the last time!)
Subscriber received value: Hello for the last time!
[Debug] : receive cancel
```

# 4. Published

```swift
final class SomeViewModel {
    @Published var name: String = "kite"
    var age: Int = 23
}

final class Label {
    var text: String = ""
}

let label = Label()
let viewModel = SomeViewModel()
print("text: \(label.text)")

viewModel.$name.assign(to: \.text, on: label)
print("text: \(label.text)")

viewModel.name = "jeongkite"
print("text: \(label.text)")
```

```
💻 # 결과

text:
text: kite
text: jeongkite
```

# 5. Foundation : URLSessionTask & Notifications & Timer

```swift
// URLSessionTask Publisher and JSON Decoding Operator
struct SomeDecodable: Decodable { }

URLSession.shared.dataTaskPublisher(for: URL(string: "https://www.google.com")!)
    .map { data, response in
        return data
    }
    .decode(type: SomeDecodable.self, decoder: JSONDecoder())

// Notifications
let center = NotificationCenter.default
let noti = Notification.Name("MyNoti")
let notiPublisher = center.publisher(for: noti, object: nil)
let subscription1 = notiPublisher.sink { _ in
    print("Noti Received")
}
center.post(name: noti, object: nil)
subscription1.cancel()

// KeyPath binding to NSObject instances
let ageLabel = UILabel()
print("text: \(String(describing: ageLabel.text))")

Just(24)
    .map { "Age is \($0)" }
    .assign(to: \.text, on: ageLabel)
print("text: \(String(describing: ageLabel.text))")

// Timer
// autoconnect 를 이용하면 subscribe 되면 바로 시작함
let timerPublisher = Timer
    .publish(every: 1, on: .main, in: .common)
    .autoconnect()
let subscription2 = timerPublisher.sink { time in
    print("time: \(time)")
}
DispatchQueue.main.asyncAfter(deadline: .now() + 5) {
    subscription2.cancel()
    print("subscription2 is canceled")
}
```

```
💻 # 결과

Noti Received
text: nil
text: Optional("Age is 24")
time: 2023-01-25 14:05:30 +0000
time: 2023-01-25 14:05:31 +0000
time: 2023-01-25 14:05:32 +0000
time: 2023-01-25 14:05:33 +0000
time: 2023-01-25 14:05:34 +0000
subscription2 is canceled
```

# 6. Scheduler

```swift
let arrPublisher = [1, 2, 3].publisher
let queue = DispatchQueue(label: "custom")
let subscription = arrPublisher
    // 무거운 작업은 Custom Thread에서 돌아가도록
    .subscribe(on: queue)
    .map { value -> Int in  // Operator
        print("transform: \(value), thread: \(Thread.current)")
        return value
    }
    // 실제로 데이터를 받을 때는 Main Thread에서 받을 수 있게끔 : ex. UI 업데이트
    .receive(on: DispatchQueue.main)
    .sink { value in
    print("Receive value: \(value), thread: \(Thread.current)")
}
```

```
💻 # 결과

transform: 1, thread: <NSThread: 0x600002182280>{number = 4, name = (null)}
transform: 2, thread: <NSThread: 0x600002182280>{number = 4, name = (null)}
transform: 3, thread: <NSThread: 0x600002182280>{number = 4, name = (null)}
Receive value: 1, thread: <_NSMainThread: 0x6000021841c0>{number = 1, name = main}
Receive value: 2, thread: <_NSMainThread: 0x6000021841c0>{number = 1, name = main}
Receive value: 3, thread: <_NSMainThread: 0x6000021841c0>{number = 1, name = main}
```

# 7. Operator : Map, Filter

```swift
// Transform - Map
let numPublisher = PassthroughSubject<Int, Never>()
let subscription1 = numPublisher
    .map { $0 * 2 }     // 값을 변형시켜준다
    .sink { value in
        print("Transformed Value: \(value)")
    }
numPublisher.send(10)
numPublisher.send(20)
numPublisher.send(30)
subscription1.cancel()

// Filter
let stringPublisher = PassthroughSubject<String, Never>()
let subscription2 = stringPublisher
    .filter{ $0.contains("a") }
    .sink { value in
        print("Filtered Value: \(value)")
    }
stringPublisher.send("abc")
stringPublisher.send("jeongkite")
stringPublisher.send("jeongkate")
stringPublisher.send("Yang!")
subscription2.cancel()
```

```
💻 # 결과

Transformed Value: 20
Transformed Value: 40
Transformed Value: 60
Filtered Value: abc
Filtered Value: jeongkate
Filtered Value: Yang!
```

# 8. Operator :  Combine Latest & Merge

```swift
// Basic CombineLatest
/*
 예를 들어 이런식으로 데이터를 보낸다면...
 "a",       "b",    "c"
      1,        2,  3,      4
 일정하지 않은 텀을 두고 데이터를 보낸다면,
      ->"a",1
            ->"b",1
                ->"b",2
 이렇게 그 시점에 각각에서 가장 최신 데이터를 보내준다.
 */
let stringPublisher = PassthroughSubject<String, Never>()
let numPublisher = PassthroughSubject<Int, Never>()
stringPublisher.combineLatest(numPublisher).sink { (string, num) in // 둘의 타입이 달라도 가능
    print("Receive: \(string), \(num)")
}
//Publishers.CombineLatest(stringPublisher, numPublisher)   // 이런식으로도 가능
stringPublisher.send("a")
numPublisher.send(1)
stringPublisher.send("b")
numPublisher.send(2)
numPublisher.send(3)
stringPublisher.send("c")
numPublisher.send(4)

// Advanced CombineLatest
let usernamePublisher = PassthroughSubject<String, Never>()
let passwordPublisher = PassthroughSubject<String, Never>()
let validatedCredentialSubscription = usernamePublisher.combineLatest(passwordPublisher)
    .map { (username, password) -> Bool in
        return !username.isEmpty && !password.isEmpty && password.count > 12
    }
    .sink { valid in
        print("is Credential valid?: \(valid)")
    }
usernamePublisher.send("jeongkite")
passwordPublisher.send("weak")
passwordPublisher.send("StrongPassW!!")
usernamePublisher.send("")

// Merge
let publisher1 = [1, 2, 3, 4, 5].publisher
let publisher2 = [1300, 411, 52].publisher

let mergePublisherSubscription = publisher1.merge(with: publisher2) // 퍼블리셔의 타입이 같아야 머지 가능
    .sink { value in
        print("Merge: subscription received value: \(value)")
    }
//let mergePublisherSubscription = Publishers.Merge(publisher1, publisher2) // 얘도 이렇게도 가능
```

```
💻 # 결과

Receive: a, 1
Receive: b, 1
Receive: b, 2
Receive: b, 3
Receive: c, 3
Receive: c, 4
is Credential valid?: false
is Credential valid?: true
is Credential valid?: false
Merge: subscription received value: 1
Merge: subscription received value: 2
Merge: subscription received value: 3
Merge: subscription received value: 4
Merge: subscription received value: 5
Merge: subscription received value: 1300
Merge: subscription received value: 411
Merge: subscription received value: 52
```

# 9. Operator : removeDup & compactMap

```swift
var subscriptions = Set<AnyCancellable>()

// removeDuplicates
// 같은 데이터가 들어올 때, 중복 데이터를 지우는 방법
let words = "hey hey there! Mr Mr ?"
    .components(separatedBy: " ")
    .publisher
words
    .removeDuplicates()
    .sink { value in
        print("removeDuplicates value: \(value)")
    }.store(in: &subscriptions)


// compactMap
// 전환을 했는데, 전환한 내용이 nil인 경우 보내지 않는 방법
let strings = ["a", "1.24", "3", "def", "45", "0.23"].publisher
strings
    .compactMap { Float($0) }
    .sink { value in
        print("compactMap value: \(value)")
    }.store(in: &subscriptions)


// ignoreOutput
// 구독을 하긴 했는데, 새로 들어오는 이벤트 데이터를 신경쓰고싶지 않을 때
let numbers = (1...10000).publisher
numbers
    .ignoreOutput()
    .sink(receiveCompletion: { print("ignoreOutput value: \($0)")}, receiveValue: { print($0) })
    .store(in: &subscriptions)


// prefix
// 여러개의 데이터가 들어올 때, 몇개만 받겠다
let tens = (1...10).publisher
tens
    .prefix(2)
    .sink { value in
        print("prefix value: \(value)")
    }.store(in: &subscriptions)
```

```
💻 # 결과

removeDuplicates value: hey
removeDuplicates value: there!
removeDuplicates value: Mr
removeDuplicates value: ?
compactMap value: 1.24
compactMap value: 3.0
compactMap value: 45.0
compactMap value: 0.23
ignoreOutput value: finished
prefix value: 1
prefix value: 2
```
