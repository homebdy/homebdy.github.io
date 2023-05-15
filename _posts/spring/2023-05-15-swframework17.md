---
layout: post
title:  "스프링 Transaction"
author: yj
category: [ Spring🌱 ]
tags: [ JAVA, SPRING ]
---


## <a href="#">트랜잭션</a>

전체 로직이 모두 다 반영되거나 모두 반영되지 않은 논리적인 작업의 묶음
- All or Nothing
- 논리적 단위로 어떤 한 부분의 작업이 완료되었어도 다른 부분의 작업이 완료되지 않은 경우 전부 취소
- 이때 작업이 완료되는것: `commit`
- 작업이 취소 되는 것: `rollback`
- 프리젠테이션 층과 비즈니스 층 사이에 놓여지는 것이 전통적인 방식
    - 프리젠테이션 층에 공개된 서비스의 메서드가 트랜잭션의 시작과 종료
**기본 원칙**

1. 원자성
- 한 트랜잭션 내의 모든 연산들이 완전히 수행되거나 전혀 수행되지 않는다.

2. 일관성
- 어떤 트랜잭션이 수행되기 전 데이터베이스가 일관된 상태를 가졌다면 트랜잭션이 수행된 이후에 데이터베이스는 또 다른 일관된 상태를 가져야 한다.

3. 고립성
- 한 트랜잭션이 데이터를 갱신하는 동안 이 트랜잭션이 완료되기 전에는 갱신 중인 데이터를 다른 트랜잭션에서 접근할 수 없다.

4. 지속성
- 일단 한 트랜잭션이 완료되면 데이터베이스에 반영한 수행 결과는 영구적이어야 한다.

### AOP를 이용한 트랜잭션 처리

AOP로 서비스에 트랜잭션 처리를 구현한 어드바이스를 적용함으로써 서비스 내부를 수정하지 않고 트랜잭션을 처리한다.
- 트랜잭션 처리는 트랜잭션 관리자와 트랜잭션 어드바이스 이용

**트랜잭션 관리자**

Spring은 데이터베이스 연동 기술과 트랜잭션 서비스 사이 종속성을 제거하고 트랜잭션 추상 계층을 이용한다.
- 데이터베이스 연동 기술에 상관없이 동일한 방식으로 트랜잭션 관리 기능 제공
- 트랜잭션 처리의 중심이 되는 인터페이스는 `PlatformTransactionManager`
- 스프링을 이용해 트랜잭션을 처리하기 위해서는 DB 연동 기술에 적합한 트랜잭션 관리지를 등록해야한다.
- 트랜잭션 구현 클래스: 트랜잭션 시작과 종료, 롤백 처리를 비롯해 트랜잭션의 정의 정보를 설정한다.
    - 종류
        1. DataSourceTransactionManager: JDBC, MYBATIS 등의 JDBC 기반의 라이브러리로 접근
        2. HibernateTransactionManager: 하이버네이트를 이용한 접근
        3. JpaTransactionManager: JPA로 접근
        4. JtaTransactionManager: JTA로 접근
- 기본적인 트랜잭션 관리자 설정 방법
    ```xml
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
    ```

### 트랜잭션 정의 정보

1. 전파(propagation) 속성: 기존 트랜잭션이 있는 상태에서 새로운 트랜잭션을 별도 생성하거나 기존 트랜잭션에 포함시켜 실행하도록 설정하기 위한 트랜잭션 전파에 대한 설정

2. 독립성(isolation) 수준: 트랜잭션 처리가 병행해서 실행될 때 각 트랜잭션의 독립성을 결정
    - 하나의 트랜잭션 수행 시 다른 트랜잭션의 연산 작업이 끼어들지 못하도록 보장하는 정도

3. 만료 시간: 트랜잭션이 취소되는 만료 시간을 초 단위로 설정

4. 읽기 전용 상태: 트랜잭션 내의 처리가 읽기 전용으로 설정
5. 롤백 대상 예외
6. 커밋 대상 예외

**@Transaction 애노테이션의 전파 속성**

2개 이상의 트랜잭션이 작동할 때 기존 트랜잭션에 참여하는 방법을 결정하는 속성

- 전파 결정
    1. 컨트롤러 1에서 서비스 1 메서드를 호출한 경우
        - 트랜잭션은 서비스 1의 메서드가 호출되었을 때 시작
    
    2. 컨트롤러 2에서 서비스 2를 호출하고 서비스 2에서 서비스 1 호출하는 경우
        - 서비스 1의 메서드가 호출되면 동시에 트랜잭션을 새로 시작할 지 아니면 원래 트랜잭션을 그대로 이어나갈 지 선택 필요 = `전파` 속성

- 종류: default = REQUIRED
    1. MANDATORY: 기존 트랜잭션이 존재하는 경우 포함되어 실행하고 아닌 경우 예외 발생
    2. NESTED: 기존 트랜잭션이 존재하면 포함되어 실행되고 없는 경우 새로운 트랜잭션 생성
        - REQUIRED 속성과 동일하지만 차이점은 SAVEPOINT로 지정한 시점까지 부분 롤백이 가능하다는 점
    3. NEVER: 기존 트랜잭션이 존재하는 경우 예외 발생, 아닌 경우 트랜잭션 없이 실행
    4. SUPPORTS: 기존 트랜잭션이 존재하면 포함해서 실행, 없으면 트랜잭션 없이 실행
    5. NOT_SUPPORTED: 기존 트랜잭션이 존재하면 트랜잭션 없이 실행, 없어도 없이 실행
    6. REQUIRED: 기존 트랜잭션이 있으면 해당 트랜잭션에 포함, 없으면 새로운 트랜잭션 생성
    7. REQUIRED_NEW: 항상 새로운 트랜잭션 실행
        - 기존 트랜잭션이 존재하면 일시중지 후 새로운 트랜잭션 완료 후 기존 트랜잭션 실행
    
**@Transactional 애노테이션의 격리(Isolation) 수준 속성**

여러 개의 트랜잭션이 동시에 실행될 때 어떻게 데이터 무결성을 보장할 지에 대한 결정
- 종류
    1. DEFAULT: DB 설정을 따른다
    2. READ_UNCOMMITED: 다른 트랜잭션에 의해 커밋되지 않은 데이터를 읽을 수 있다.
    3. READ_COMMITED: 다른 트랜잭션에 의해 커밋된 데이터를 읽을 수 있다.
    4. REPEATABLE_READ: 트랜잭션 내 동일 필드에 대해 처음 읽어 온 데이터와 두 번째 읽어 온 데이터가 동일한 값을 갖도록 보장
    5. SERIALIZABLE: 가장 높은 격리 수준으로 동일한 데이터에 대해 동시에 2개 이상의 트랜잭션이 수행될 수 없다.

**그 외 트랜잭션 정의 정보**

1. read-only: 트랜잭션 처리가 읽기 전용으로 설정
    - true 이면(읽기 전용) insert, update, delete 실행시 예외 발생
2. rollbackFor: 어느 예외가 발생할 경우 롤백할 지 설정할 수 있음
3. noRollbackFor: 특정 예외가 발생할 경우 Rollback 처리되지 않음
4. Timeout: 트랜잭션이 취소되는 만료 시간을 초 단위로 설정

**@Transactional 기본 롤백**

RuntimeException과 Error에 대해서만 롤백
- Exception에 대해서는 롤백하지 않는다.
- Error: 비정상적인 상황이 발생한 경우
- Exception: 개발자의 실수

### 트랜잭션 기능 사용 방법

1. 선언적 트랜잭션 처리: 미리 선언된 규칙에 따라 트랜잭션을 제어
    - xml, Annotation 설정 방식
    - 소스 코드 상 트랜잭션 처리 메서드를 호출하므로 복잡하고 가독성이 떨어짐
    ```java
    @Autowired
    private PlatformTransactionManager txManager;
        public void updateMemberList(StudentVO student, StudentVO student) throws Exception {
            DefaultTransactionDefinition def = new DefaultTransactionDefinition();
            def.setPropagationBehavior( TransactionDefinition.Propagation.REQUIRED );
            TreansactionStatus status = txManager,getTransaction(def);

            txManager.commit(commit); // 정상 완료시
            txManager.rollback(rollaback); // 예외 발생시
        }
    ```
    - xml 방식: \<tx:advuce />태그를 통해 어드바이스 설정
    - Annotation 방식: @Transactional을 통해 처리가 필요한 클래스와 메서드에 설정