> 오늘은 진짜 뭘 써야할지 모르겠어서..ㅋㅋ
책에서 읽은 잡다한 것들에 대해 적어본다.
> 

# Swift의 데이터 타입

## Int

### Int의 종류

- 부호에 따라
    - Int : 0, 음&양의 정수 표현 가능
    - UInt : 0을 포함한 양의 정수 표현 가능
- 크기에 따라
    - Int8, UInt8
    - Int16, UInt16
    - Int32, UInt32
    - Int64, UInt64
    
    → 어떤 Int, UInt가 될지는 시스템 아키텍처에 따라 달라진다.
    
    ex. 32비트 아키텍처에서는 Int32가 Int, UInt32가 UInt 타입으로 지정 (64면 동일하게 Int64, UInt64가..)
    

### 표현 가능 범위 알아보는 방법

- 최솟값 : Int.min, UInt.min
- 최댓값 : Int.max, UInt.max

![1](https://user-images.githubusercontent.com/75439868/214008325-9b4ef0fc-e3c2-49ca-aeac-82b984e95cdd.png)

### Int와 UInt의 적절한 사용 방법

![2표현범위](https://user-images.githubusercontent.com/75439868/214008353-e54f9409-3776-40c0-aef1-1481a4137e56.png)

- 기본적으로 Int 사용
- 특히 Int와 UInt 모두 사용 가능한 경우(연두색 범위) Int 사용

## Float와 Double

- 둘 다 부동소수점을 사용하는 실수
- 부동소수 타입
- Float
    - 32비트
    - 6자리까지 표현 가능
- Double
    - 64비트
    - 최소 15자리 이상 표현 가능

→ 기본적으로 데이터 타입 생략시, 컴파일러는 Double로 실수의 타입을 지정

![3](https://user-images.githubusercontent.com/75439868/214008403-5607186e-2874-4b36-8e1e-008bc29b2cd8.png)

## String

### append(), + 연산자 : 문자열 이어붙이기

```swift
var string: String = String()
string = "안녕"
string.append("하세요!")
print(string)              // 안녕하세요!
string = "안녕" + "하세요!"
print(string)              // 안녕하세요!
```

### count : 문자열 길이 구하기

```swift
"hello".count    // 5
"".count         // 0
```

### isEmpty : 비어있는지 확인하기

```swift
"hello".isEmpty    // false
"".isEmpty.        // true
```

→ isEmpty가 있으니 .count == 0이 아니라 .isEmpty를 사용하자!

### 유니코드 변환

![4](https://user-images.githubusercontent.com/75439868/214008441-6c3a4002-4090-41da-8c76-6e368488a2c6.png)

여기서 똑같은 숫자를 넣었는데 다른 결과가 나오는 이유

33번 라인에서는 16진수를 사용하기 때문~~

### hasPrefix, hasSuffix : 접두어, 접미어 확인

```swift
"안녕하세요".hasPrefix("안녕")     // true
"안녕하세요".hasPrefix("요")      // false
"안녕하세요".hasSuffix("안녕")    // false
"안녕하세요".hasSuffix("요")     // true
```

### uppercased(), lowercased() : 대소문자 변환

```swift
"My name is Jeongkite!".uppercased()    // "MY NAME IS JEONGKITE!"
"My name is Jeongkite!".lowercased()    // "my name is jeongkite!"
```

## Any와 AnyObject 그리고 nil 또 Never

### Any

- 모든 데이터 타입 사용 가능하다는 의미
- 사용시, 해당 변수 혹은 상수에 어떤 종류의 데이터 타입이든 할당 가능

### AnyObject

- 클래스의 인스턴스만 할당 가능
- 어떤 클래스의 인스턴스든 할당 가능

### nil

- 타입이 아닌 **‘없음’**을 나타내는 Swift의 키워드
- 접근시 Null Point Exception 런타임 오류 발생

### Never

- 비반환 함수의 반환타입으로 사용
- 비반환 함수
    - 오류 던지기
    - 시스템 오류 보고
    
    등의 일을 한 후 프로세스 종료
    
    ex. fatalError 함수
