# Swift : 편리한 문서화를 돕는 다양한 마크업 주석 활용

그동안 적당히 필요한 부분 드래그해서 `cmd` + `/` 만 눌러 주석을 달아왔는데,
이번 기회에 마크업 문법을 활용한 주석 작성법을 정리해보고자 한다.

*들어가기에 앞서 퀵헬프 간단 소개*

- Xcode의 기본 기능.
- 사용 방법
    - 궁금한 부분에 마우스를 올리고 `option` 키를 누르면 `?` 표시가 뜬다.
    - 그 상태로 클릭 하면 말풍선 형태로 도움말이 나온다.
    - 도움말 우측 하단의 `Open in Developer Documentation`를 누르면 해당 내용의 개발자 문서를 바로 볼 수 있다.

 → 직접 작성한 변수나 함수, 클래스에도 마크업 문법에 맞게 주석을 작성하면 퀵헬프 기능을 사용할 수 있다!

## 기본적인 주석 작성 방법

### 1. 한 줄 주석

- 맨 앞에 슬래시 두 개 ( `//` ) 사용
- `cmd` + `/` 를 이용해 주석을 만들 때에도 이 방식이 사용됩니다.

```swift
// 한 줄 주석이랍니다~
```

### 2. 여러 줄 주석

- 맨 앞에 슬래시와 별표 ( `/*` ) 맨 끝에 별표와 슬래시 ( `*/` ) 사용

```swift
/*
이 사이의 내용은 모두 주석 처리 된답니다~~
유후후~
야하하하하~~
룰루~
*/

/* 한 줄 주석에도 사용할 수 있어요! */
```

### 3. 주석 중첩

- 다른 프로그래밍 언어들과 다르게, Swift는 주석 안에 또 주석을 넣을 수 있습니다.
- 한 줄, 여러 줄 구분 없이 가능합니다.

```swift
/*
여기도 주석
// 저기도 주석
/* 주석이 콸콸콸~! */
*/
```

---

## 퀵헬프에 표시되는 주석 작성 방법

이제 본격적으로 문서화를 위한 마크업 문법을 활용한 주석 작성 방법을 알아봅시다.

### 0. 퀵헬프를 위한 주석 작성 방법

퀵헬프에 작성한 주석 내용이 뜨도록 하기 위해서는

- 한 줄 주석
    - 슬래시 세 개 ( `///` ) 사용

```swift
/// 퀵헬프를 띄우고 싶은 내용 바로 위에 슬래시 3개를 사용해 주석을 달아보세요!
/// 한 줄 주석을 여러개 사용해 여러 줄을 표시할 수도 있습니다.
```

- 여러 줄 주석
    - 시작 부분에 슬래시 하나와 별표 두 개, 마지막엔 별표 하나와 슬래시 하나 ( `/**`  `*/` ) 사용
    - `/**` `**/` 를 사용해도 가능은 합니다.

```swift
/**
하지만 여러줄을 표시하고자 할 때는 
이걸 사용하는게 더 편하겠죠?
근데 사실 ///를 사용해도 줄바꿈 할 때 자동으로 다음 줄에도 ///를 추가해줘서 상관은 없답니다!
보기 편한 방법으로 주석을 작성해보세요!
*/
```

- 문서화 주석 자동 생성 단축키
    - Xcode 상단 메뉴 중 Editor > Structure > Add Documentation (단축키 : `opt` + `cmd` + `/` )
    - 문서화 하고자 하는 부분에 커서를 가져다 두고 위 메뉴를 선택해보세요.
    
    ![1](https://user-images.githubusercontent.com/75439868/213867070-fc2ec7ac-1190-48af-940d-a1a1a563fd6f.png)
    
    ```swift
    - 클래스 혹은 변수, 상수의 경우
    /// *<#Description#>*
    
    - 함수의 경우
    /// *<#Description#>*
    /// - Parameters:
    ///   - param1: *<#*param1 *description#>*
    ///   - param2: *<#*param2 *description#>*
    ```
    
    - 자동으로 위와 같은 주석 코드 조각을 만들어준답니다!
    - 작성해야 하는 부분만 `tap` 키로 넘나들며 편하게 작성할 수 있어요.

### 1. 지정된 키워드를 사용한 강조

****Discussion**** 외의 내용을 적고 싶다면

### **Parameter**

- `- Parameter` or `- Parameters:` 키워드 사용
- 파라미터가 있는 함수에서 자동 주석을 추가한다면 해당 함수가 가진 파라미터를 자동으로 정리해줍니다!
- 파라미터가 한 개라면
    
    ```swift
    /// 파라미터가 하나인 경우 줄바꿈 없이 바로 적어줄 수 있습니다.
    /// - Parameter string: 문자열
    ```
    
    ![2](https://user-images.githubusercontent.com/75439868/213867094-4817e0d6-5093-48e7-bf6a-0e282565b49d.png)
    

- 파라미터가 여러 개라면
    
    ```swift
    /// 파라미터가 두 개 이상인 경우 파라미터별로 `-` 및 줄바꿈이 필요합니다.
    /// - Parameters:
    ///   - string: 문자열
    ///   - int: 정수
    ```
    
    ![3](https://user-images.githubusercontent.com/75439868/213867117-926c077f-6f43-47b1-bbe2-8e63cb157e92.png)
    

### **Throws와 Returns**

- 예외처리에 대해 작성하고 싶다면
    - `- Throws:` 사용
- 반환값에 대해 작성하고 싶다면
    - `- Returns:` 사용

```swift
/**
 - Throws: 이걸 던져?
 - Returns: 이걸 반환해??
 */
```

![4](https://user-images.githubusercontent.com/75439868/213867130-2317607f-7fe3-458c-b6d5-9f6e352e61ce.png)

이외에도 Attention, Author, Authors, Bug, Complexity, Copyright, Custom, Callout, Date, Example, Experiment, Important, Invariant, Note, Precondition, Postcondition, Remark, Requires, See, Also, Since, Version, Warning과 같은 키워드를 사용하면 Discussion 내에서 내용을 강조할 수 있습니다.

### 2. 원형 글머리 기호

- `-` , `*` , `+` 중 하나를 사용
- 어떤 기호를 사용하던, 동일하게 원형으로 표시됩니다.
- 들여쓰기를 이용한 계층 표현도 가능합니다.

```swift
/// - '-' 를 이용한 주석입니다.
/// * '*' 를 이용한 주석입니다.
/// + '+' 를 이용한 주석입니다.
///     - 계층도 표현 가능합니다.
///         - 한 번 더
///             - 마지막으로!
```

![5](https://user-images.githubusercontent.com/75439868/213867151-7c7e5a27-27ea-456b-a3fc-494c125e0b2f.png)

### 3. 번호 글머리 기호

- `숫자` + `.` 사용
- 아무 숫자나 넣어도 순서에 맞게 붙여줍니다.
- 들여쓰기를 해도 기호의 종류가 바뀌진 않습니다.

```swift
/// 1. 숫자를 붙여줄 수 있습니다.
/// 3. 숫자는 아무렇게나 붙여도
/// 7. 순서에 맞게 1 부터 나타납니다.
///     1. 들여쓰기를 해볼까요?
///     4. 이렇게!
/// a. 숫자만 가능합니다.
```

![6](https://user-images.githubusercontent.com/75439868/213867163-e7368ebf-96bf-498a-8119-c566672e1abb.png)

### 3. 줄 바꿈

- 줄 바꿈
    - 텍스트 사이에 `빈 줄` 사용

```swift
/**
 맨 위에 줄은 Summary로 표시됩니다.
 
 줄바꿈을 하고싶다면
 엔터를 한 번이 아니라
 
 두 번 해보세요
 - 글머리 기호는 자동으로 줄바꿈이 된답니다
*/
```

![7](https://user-images.githubusercontent.com/75439868/213867170-d6f373cd-7ea5-4ffb-be9b-3cb55d20befb.png)

- ~~문단 바꿈~~
    
    이전에는 대시 세 개 이상 ( `---` )을 사용해 문단 사이에 긴 줄을 표시할 수 있었다고 하는데,
    
    지금은 사용할 수 없습니다. 사용하면 더 이상 퀵헬프에 나타나지 않아요!
    

### 5. 텍스트 꾸밈

- 굵은 글씨
    - 별표 혹은 언더바 두 개( `**` or `__` )를 굵게 표시할 내용의 시작과 끝에 사용
- 기울인 글씨
    - 별표 혹은 언더바 한 개( `*` or `_` )를 기울일 내용의 시작과 끝에 사용
- 코드체
    - 백틱 ( ``` )을 코드와 같은 글씨체로 표시할 내용의 시작과 끝에 사용
- 위 기호는 중첩해 적용할 수 없습니다.

```swift
/// 아래와 같이 글꼴을 바꾸어 나타낼 수 있습니다.
///
/// **굵게 표시**
///
/// *기울여 표시*
///
/// `font`
///
/// 기본 font
```

![8](https://user-images.githubusercontent.com/75439868/213867180-3a0f163b-a91c-44f0-ae59-1f3c2556117b.png)

### 6. 코드 블럭 삽입

- 코드 부분에 들여쓰기
- 시작과 끝에 백틱 세 개 이상( ````` ) 사용

```swift
/// 아래와 같이 코드블럭을 삽입할 수 있습니다.
///
/// 다른 텍스트와 다르게 들여쓰기 할 경우,
///
/// 이 경우 **줄바꿈도 필요**합니다.
///
///     let code: String // 주석도 가능!
///
/// 백틱을 세 개 이상 연달아 사용할 경우,
///
/// 이때는 줄바꿈 하지 않아도 가능합니다.
/// ```
/// let code: String    // 역시 주석도 가능~
///
/// private func makeCodeBlock() {
///     print("This is code block!")    // 줄바꿈이나 들여쓰기도 그대로 반영됩니다.
/// }
/// ```
```

![9](https://user-images.githubusercontent.com/75439868/213867186-c1f79bf4-edc0-476a-9cb2-7338854ddfdc.png)

---

📌 위에 작성한 내용 외에도 사진 및 동영상 추가, 링크 추가 등 다양한 마크업 문법을 활용할 수 있으니, 더 궁금한 분들은 [공식문서를 참고](https://developer.apple.com/library/archive/documentation/Xcode/Reference/xcode_markup_formatting_ref/index.html#//apple_ref/doc/uid/TP40016497-CH2-SW1)해주세요!