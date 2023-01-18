사실 REST API를 정리해보려고 했는데,

이것부터 하는게 더 좋을 것 같아 나중으로 미룬다.

# API : Application Programming Interface

- 정의 및 프로토콜 집합을 사용하여 두 소프트웨어 구성 요소가 서로 통신할 수 있게 하는 메커니즘
- 응용 프로그램에서 사용할 수 있도록, 운영체제나 프로그래밍 언어에서 제공하는 기능을 제어할 수 있게 해주는 인터페이스
- 한 프로그램에서 다른 프로그램으로 데이터를 주고받기 위한 방법
- 키보드


---

![api](https://user-images.githubusercontent.com/75439868/213181228-b5527e66-1cc7-47e0-b72c-e19e6411aebb.png)

### 서버와 클라이언트가 데이터를 주고받기 위한 정확한 방법!

- 요청과 응답을 사용하여 두 애플리케이션이 서로 통신하는 방법을 정의
- API 문서에는 개발자가 이러한 요청과 응답을 구성하는 방법에 대한 정보가 담겨있음

## API의 유형

- 프라이빗 API
    - 기업 내부에서만 사용
- 퍼블릭 API
    - 일반 사람들에게 공개되며 누구나 사용 가능
    - 권한이나 비용, 요청 횟수에 대한 제약이 있을 수 있음
- 파트너 API
    - B2B
    - 허용된 외부 개발자는 접근 가능
- 복합 API
    - 두 개 이상 서로 다른 API를 결합해 요구사항 처리

### 모든 프로그램은 API를 가질 수 있음

그래서 구글 지도 API를 써서 지도 정보 사용하고,

브라우저 API 사용해서 카메라나 마이크 접근권한 가져와서 사용하고 그런게 가능~

## API의 종류

### REST (2000~)

- 아키텍처 스타일
- GET, POST, DELETE, PUT, PATCH를 사용해 데이터 CRUD 처리
- 보통 JSON 사용
- 캐시 사용 가능

언제 쓰면 좋을까?

→ 다른 모든 경우에 속하지 않는 일반적인 경우

### SOAP (1999~)

- Simple Object Access **Protocol**
- HTTP method중 POST만을 이용해 데이터의 CRUD를 처리
- 높은 보안성 : 금융 정보 등 민감한 데이터를 주고받을 때 일반적으로 사용
- XML 사용
- 캐시 사용 불가

언제 쓰면 좋을까?

→ 보안이 중요한 경우, 예를 들면 금융이나 사내 프로그램

### GraphQL (2015~)

- 페이스북 개발
- API를 위한 쿼리 언어
- 클라이언트가 요청한 만큼의 데이터 제공이 우선 : 정확한 요청이 초점
- 단일 API 호출로 다양한 데이터 요청이 가능

### gRPC (2016~)

- Google이 만든 Remote Procedure Calls
- 클라이언트가 데이터를 요청하고 응답받는게 아닌,
마치 로컬인 것 처럼 직접 서버에 있는 함수 호출 가능
- HTTP/2 사용
- 권한은 클라이언트가, 처리 및 계산은 서버가 → 클라에 드는 리소스가 적다
- 모든 스트리밍 조합 지원 (단항, 서버-클라, 클라-서버, 양방향)

언제 쓰면 좋을까?

→ 스트리밍, 마이크로 서비스, IoT 장치

---

### 참고

[API란 무엇인가요? - API 초보자를 위한 가이드 - AWS](https://aws.amazon.com/ko/what-is/api/)

[gRPC 서비스와 HTTP API 비교](https://learn.microsoft.com/ko-kr/aspnet/core/grpc/comparison?view=aspnetcore-7.0)

[GraphQL 개념잡기](https://tech.kakao.com/2019/08/01/graphql-basic/)

[API란? What is API? API 뜻, API 개발 방식, API 연동, Web API](https://www.redhat.com/ko/topics/api/what-are-application-programming-interfaces)

[API란? - SOAP, REST, GraphQL, gRPC 비교 - 총정리](https://bangu4.tistory.com/167)

[Restful vs gRPC vs GraphQL](https://giljae.com/2022/08/05/Restful-vs-gRPC-vs-GraphQL.html)
