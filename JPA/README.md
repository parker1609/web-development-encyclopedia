# JPA 관련 질문

## JPA(Java Persistence API)란?
자바 ORM 기술에 대한 API 명세로, Java에서 제공하는 API입니다. 주의할 점은 JPA는 라이브러리가 아니라 단순히 **ORM을 사용하기 위한 인터페이스 모음**이라는 것입니다. 따라서 JPA를 구현하는 Hibernate, EclipseLink, DataNucleus 등과 같은 ORM 프레임워크를 사용해야합니다.

![JPA 모습](./images/jpa.png)
출처: <https://velog.io/@adam2/JPA%EB%8A%94-%EB%8F%84%EB%8D%B0%EC%B2%B4-%EB%AD%98%EA%B9%8C-orm-%EC%98%81%EC%86%8D%EC%84%B1-hibernate-spring-data-jpa>

### ORM(Object-Relation Mapping)
ORM은 객체와 데이터베이스 테이블을 매핑해주는 기술입니다. 즉, 자바와 같은 언어에서 **객체가 데이터베이스의 테이블이 될 수 있도록 매핑** 시켜주는 것을 의미합니다. 따라서 자바 언어에 국한된 것이 아닌 여러 언어에서 사용하는 기술입니다.

ORM의 장점은 쿼리를 직접 작성하지 않고 메서드와 같은 코드를 통해 데이터베이스의 데이터를 조작할 수 있습니다. 따라서 생산성이 매우 높아집니다. 반면에 복잡한 데이터를 다루기에는 실제 쿼리보다는 성능이 안좋은 단점이 있습니다. 일반적으로 간단한 CRUD는 JPA를 사용하며, 복잡한 조회 쿼리와 같은 것은 실제로 쿼리를 작성하는 것이 좋습니다.

#### ORM VS SQL Mapper
데이터베이스를 다루는 Persistance Framework는 크게 ORM과 SQL Mapper로 나뉩니다.

ORM은 객체와 데이터베이스 테이블을 매핑하여 SQL을 직접 작성할 필요가 없습니다.

SQL Mapper는 객체와 SQL을 매핑하는 기술로 SQL은 직접 작성해야 합니다. 그리고 ORM은 객체간의 관계를 중점적으로 생각하지만, SQL Mapper는 단순히 SQL 쿼리 결과와 객체를 매핑해주는데 중점을 둡니다. SQL Mapper는 대표적으로 iBatis, MyBatis, Oracle SQLJ 등이 있습니다.

#### ORM이 나온 배경
ORM이 만들어진 계기는 크게 두 가지가 있습니다.
- 유사한 CRUD 쿼리의 단순 반복 작업
- 객체지향 프로그래밍과 관계형 데이터베이스 간의 패러다임 불일치

유사한 CRUD 쿼리의 단순 반복 작업은 말 그대로 JDBC API, SQL Mapper를 사용하면 쿼리를 직접 만들어야 합니다. 테이블이 많아질수록 이는 단순 반복 작업만 많아지고 생산성이 매우 떨어집니다.

객체지향 프로그래밍이 추구하는 것은 추상화, 캡슐화, 상속, 다형성 등을 통한 객체를 중심으로 유연한 코드를 작성하는 것입니다. 그에 반에 관계형 데이터베이스는 데이터 중심으로 테이블간의 관계(모델링)가 중심이며, 데이터의 중복을 없애는 것이 중요합니다. 소프트웨어는 사용자를 위해 유연한 코드를 작성하는 객체지향 프로그래밍이 중요하지만 데이터베이스와의 패러다임 불일치로 인해 포기해야할 부분이 많았습니다.

이를 해결하기 위해 객체지향 프로그래밍에서 상속 관계, 객체 간의 관계 등을 데이터베이스의 테이블로 매핑해주는 ORM 기술을 만들었습니다.

#### ORM(Hibernate) 장단점


### JDBC
JDBC는 자바 언어로 다양한 종류의 관계형 데이터베이스에 접속하고 SQL문을 수행하여 처리하고자 할 때 사용되는 표준 SQL 인터페이스 API입니다. JDBC는 자바 표준에서 지원하는 데이터베이스 접속 방법이므로 ORM, SQL Mapper 등 어떤 Persistance Framework 든지 상관없이 **데이터베이스에 접근하기 위해서는 내부적으로 JDBC API를 사용**해야 합니다.

![JDBC 과정](./images/jdbc.png)
출처: <https://velog.io/@adam2/JPA%EB%8A%94-%EB%8F%84%EB%8D%B0%EC%B2%B4-%EB%AD%98%EA%B9%8C-orm-%EC%98%81%EC%86%8D%EC%84%B1-hibernate-spring-data-jpa>

### Spring Data JPA
Spring Data JPA는 Spring에서 제공하는 모듈 중 하나로, 개발자가 좀 더 JPA를 쉽고 편하게 사용하기 위해 만들어진 것입니다. 이는 **JPA를 한 단계 추상화한 Repository라는 인터페이스를 제공하는 함으로써 이루어집니다.** 개발자는 `Repository` 인터페이스를 구현하여 규칙에 따라 필요한 메서드를 구현하면 Spring에서 해당 메서드에 필요한 Bean을 등록하여 JPA가 동작하도록 합니다.

![스프링 부트에서 JPA 모습](./images/spring_boot_jpa_structure.png)
출처: <https://suhwan.dev/2019/02/24/jpa-vs-hibernate-vs-spring-data-jpa/>

Spring Data JPA를 사용하기 때문에 기존 JPA에서 사용하는 `EntityManager`를 구현할 필요가 없어졌습니다.(`Repository` 내부에 구현되어 있습니다.) 그리고 또 하나의 장점은 Spring Framework의 모듈이므로 쉽게 다른 저장소 모듈로 쉽게 교체할 수 있습니다.


## Q. 영속성 컨텍스트
영속성 컨텍스트는 JPA에서 엔티티를 관리하는 논리적인 개념입니다. 실제로 영속성 컨텍스트를 수행하는 것은 `EntityManager`입니다.

### Entity
엔티티는 데이터베이스 테이블과 매핑되는 객체를 말합니다.

### `EntityManagerFactory`, `EntityManager`
`EntityManagerFactory`는 말 그대로 `EntityManager`를 만들어주는 공장 역할을 합니다. 이는 쓰레드 세이프하지만 애플리케이션당 하나의 `EntityManagerFactory`만 있어야 합니다.

`EntityManager`는 엔티티를 실질적으로 관리하는 역할이며, JPA의 핵심입니다. `EntityManagerFactor`는 트랜잭션 하나당 하나의 `EntityManager`를 생성하며, 동시성을 제어하지 않으므로 `EntityManager`는 쓰레드간 공유할 수 없습니다. 이는 여러 트랜잭션 서로간 영향을 주면 안되기 때문이기도 합니다. 따라서 쓰레드당 하나의 `EntityManager`를 생성하고, 쓰레드가 끝나면 해당 `EntityManager`도 종료됩니다.

### 영속성 컨텍스트 생명주기

![영속성 컨텍스트 생명주기](./images/persistance_context_lifecycle.png)

#### 비영속

