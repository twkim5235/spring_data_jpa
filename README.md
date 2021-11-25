# Spring Data JPA

## 주요 라이브러리

- 핵심 라이브러리
    - 스프링 MVC
    - 스프링 ORM
    - JPA, 하이버네이트
    - Spring Data JPA
- 기타 라이브러리
    - H2 데이터베이스 클라이언트
    - 커넥션 풀: 부트 기본은 HikariCP
    - 로깅 SLF4J(인터페이스) & LogBack(구현체)
    - 테스트

- 테스트 라이브러리
    - junit: 테스트 프레임워크, 스프링부트 2.2부터 junit5('jupiter') 사용
    - Mockito: 목 라이브러리
    - Assertj: 테스트 코드를 좀더 편하게 작성을 도와주는 라이브러리
    - spring-test: 스프링 통합 테스트 지원

### 공통 인터페이스 설정

**JavaConfig 설정 - 스프링 부트 사용시 생략 가능**

~~~java
@EnableJpaRepositories(basePackages = "package")
~~~

- 스프링 부트 사용 시 @SpringBootApllication의 위치를 기준으로 config 설정 됨(해당 패키지와 하위 패키지 인식)
- 만약 위치가 달라지면 @EnableJpaRepositories 필요로 됨.



**스프링 데이터 JPA가 구현클래스를 대신 생성해 준다.**

~~~
                        |<interface> ItemRepository|
                                    ↑
|Spring Data JPA| ↘︎(생성)            ↑
                     ↘              ↑
                       ->  |ItemRepository 구현 클래스|
~~~

- 스프링 데이터 JPA가 애플리케이션 로딩 시점에 해당 Repository의 구현클래스를 만들어준다.
- @Repository 어노테이션 생략 가능
  - 컴포넌트 스캔을 스프링 데이터 JPA가 자동으로 처리
  - JPA 예외를 스프링 예외로 변환하는 과정도 자동으로 처리



**공통 인터페이스 구성**

![공통인터페이스 구성도](./picture/JpaRepository_상속_구조도.png)

**제네릭 타입**

- T: 엔티티
- ID: 엔티티의 식별자 타입
- S: 엔티티와 그 자식 타입



**주요 메서드**

- save(S): 새로운 엔티티는 저장하고 이미 있는 엔티티는 병합한다.
- delete(T): 엔티티 하나를 삭제한다. 내부에서 EntityManager.remove() 호출
- findById: 엔티티 하나를 조회한다. 내부에서 EntityManager.find() 호출
- getOne(ID): 엔티티를 프록시로 조회한다. 내부에서 EntityManger.getReference() 호출
- findAll(..): 모든 엔티티를 조회한다. 정렬(sort)이나 페이징(Pageable) 조건을 파라미터로 제공할 수 있다.



JpaRepository는 대부분의 공통메서드를 제공한다.