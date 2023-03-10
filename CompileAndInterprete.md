## Compile과 Interpret?

### 이게 왜 필요해?

사실 컴퓨터는 0과 1만 직접 알아들을 수 있다! 그렇다고 우리가 0과 1로 개발하긴 너무 어려우니까… 중간에 통역사를 끼고 좀 더 편하게 개발할 수 있도록 고급 언어가 만들어졌다.

- 저급 언어 **(Low-Level Language)**
    - 기계 중심의 언어
    - 빠른 실행속도
    - 예시 : 기계어, 어셈블리어
- 고급 언어 **(High-Level Language)**
    - 사람 중심의 언어
    - 실행을 위해 **번역**이 필요
    - 예시 : C, Python, Swift, Java

→ 여기서 고급언어를 통역하는 방식이 크게 두 가지 있는 것!

### 컴파일(Compile)

to compile : pile together = 차곡차곡 쌓는다

→ 컴파일러는 전체 코드를 차곡차곡 쌓아서 한 번에 번역하고 실행 프로그램을 내놓는다. CPU는 이를 실행한다.

- 말하는 것을 처음부터 끝까지 다 듣고 한 번에 바꿔주는 방식
- 프로그램 실행을 준비하는데에 시간이 좀 더 걸린다.
- 이후 빠르고 효율적으로 실행된다.
- 장점
    - 번역하는데는 준비 시간이 조금 걸리지만, 실행 속도가 빠르다
- 단점
    - 오류를 수정하면 처음부터 다시 전체 코드를 컴파일 해야한다.

### 인터프리트(Interpret)

inter : between = 사이

→ 인터프리터는 항상 컴퓨터와 프로그램 사이에 있으면서 코드를 한 줄씩 번역하고 CPU는 이것을 번역되는 즉시 바로 한 줄씩 실행한다.

- 한 마디 할 때마다 동시통역 해주는 방식
- 느리게 실행된다
- 바로 시작한다
- 실행되는 것을  바로 볼 수 있다
- 장점
    - 중간에 오류가 있다면 이를 인지해 그 부분만 수정해 바로 다시 번역할 수 있다.
- 단점
    - 작은 단위의 코드를 나누어 계속 번역하므로 시간이 오래 걸린다. 번역하는 동안 CPU는 기다려야 한다.

## 컴파일 언어

: 컴파일 방식을 사용하는 언어

- C, Java

## 인터프리터 언어

: 인터프리트 방식을 사용하는 언어

- Python, JavaScript

## 스크립트 언어

위 둘과는 언어의 분류 방식 자체가 다르다!

컴파일, 인터프리터 언어 : 동작 방식에 따른 언어 분류 방법

스크립트 언어 : 응용 프로그램을 제어하기 위해 사용하는 언어!!!

- JavaScript : 웹 브라우저에서 구동
- ActionScript : Adobe Animate, Flash를 제어하기 위해 사용
