# REST API와 Mock API 실습 정리

## Mock API의 필요성

프론트엔드와 백엔드는 API를 통해 데이터를 주고받으며 동작한다. 하지만 실무에서는 백엔드 API 개발 완료를 기다리지 않고 프론트엔드 개발을 진행해야 한다. 이때 Mock API를 활용하면 다음과 같은 이점이 있다.

- **독립적인 개발 진행**: 백엔드 일정과 무관하게 프론트엔드 개발을 병행할 수 있다
- **빠른 프로토타이핑**: 실제 DB 없이도 데이터 연동된 프로토타입을 신속하게 제작할 수 있다
- **효율적인 테스트**: 에러 응답 등 다양한 예외 상황을 시뮬레이션하여 테스트할 수 있다

## REST API의 개념

REST(Representational State Transfer)는 웹의 장점을 최대한 활용하는 아키텍처 스타일이다. 자원을 URI로 구분하고 상태를 주고받는 방식으로 동작한다.

### REST의 3가지 핵심 구성 요소

1. **자원(Resource)**: API가 다루는 대상 (예: users, posts, products)
2. **행위(Verb)**: HTTP Methods를 통한 자원에 대한 동작
3. **표현(Representation)**: 자원의 상태를 나타내는 형식 (주로 JSON)

### CRUD와 HTTP Methods 매핑

| CRUD   | HTTP Method | 역할         |
| ------ | ----------- | ------------ |
| Create | POST        | 새 자원 생성 |
| Read   | GET         | 자원 조회    |
| Update | PUT / PATCH | 자원 수정    |
| Delete | DELETE      | 자원 삭제    |

**PUT vs PATCH**: PUT은 자원 전체를 교체하며, PATCH는 일부만 수정한다.
