# RxSwift Subject

## Observable

- create할 때 onNext로 데이터 발행
- 이후에 따로 데이터 발행하기 힘들다

→ 이를 보완하기 위해 Subject 사용

## Subject

- 생성 이후에도 onNext 메소드를 호출해 값을 발행할 수 있다

### PublishSubject

- subscribe 이후에 발행된 값을 보내준다

### BehaviorSubject

- 기본값을 가진다
- subscribe 되자마자, 아직 발행된 데이터가 없어도 기본적으로 가지고있던 값을 내려준다.
- 새로운 subscribe가 생기면 가장 최근 발행된 데이터를 내려준다.

### AsyncSubject

- 여러 subscribe가 생기고, 데이터가 발행되더라도 데이터를 내려주지 않는다.
- complete 된 시점에 모든 구독자에게 마지막으로 발행된 데이터를 내려준다.

### ReplaySubject

- subscribe를 늦게 하더라도, 앞전에 발행된 데이터를 subscribe한 시점에 모두 내려준다.

[더 자세한 내용은 공식문서 참고](https://reactivex.io/documentation/subject.html)
