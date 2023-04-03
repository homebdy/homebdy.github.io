---
layout: post
title:  "스프링 JDBC"
author: yj
category: [ Spring🌱 ]
tags: [ JAVA, SPRING ]
---

## <a href="#">데이터 액세스 층</a>

데이터 액세스 처리를 비즈니스 로직 층에서 분리하는 것

#### DAO

데이터 취득과 변경에 데이터 처리를 DAO 오브젝트로 분리하는 패턴

- 데이터 액세스 기술: JDBC, Hybernate, MyBatis, JPA등으로 구현 가능
- DAO 구현에서 스프링의 역할: 데이터 액세스 기술을 쉽게 사용하기 위한 연계 기능 제공

**데이터 소스**

데이터 액세스 기술과 상관 없이 데이터베이스의 접속을 관리해주는 인터페이스
- 업무용 어플리케이션은 `커넥션 풀`에 의해 커넥션 오브젝트 재사용
- 구현 방식
    1. 서드 파티가 제공하는 데이터 소스 사용: Apache Commons DBCP
        - 설정 방식: applicationContext.xml 파일에 정리
        
        ```xml
        <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
            <property name="driverClassName" value="com.mysql.cj.jdbc.Driver" />
            <property name="url" value="jdbc:mysql://127.0.0.1:3306/springdb?
            allowPublicKeyRetrieval=true&amp;useUnicode=true&amp;characterEncoding=utf8&amp;
            useSSL=false&amp;serverTimezone=UTC" />
            <property name="username" value="spring" />
            <property name="password" value="passwd" />
            <property name="maxActive" value="5" />
        </bean>
        ```
    2. 애플리케이션 서버가 제공하는 데이터 소스: Tomcat, Oracle WebLogic, IBM WebSphere 등
    3. 임베디드 데이터베이스가 제공하는 스프링 지원 데이터 소스: H2, HSQLDB, Aparche Derby 등

## <a href="#">스프링 JDBC</a>

**JDBC의 문제점**

1. 대량의 소스 코드 기술
2. 다양한 에러 원인을 파악하기 위한 코딩 필요
3. 데이터베이스 제품마다 에러 코드가 달라 코드 일관성 유지가 어려움

**Spring JDBC**

JDBC를 래핑한 API를 제공해 소스코드 단순화
- JDBC 직접 사용 시 발생하는 많은 코드를 은닉해준다.
- 스프링 JDBC가 제공하는 중요 탬플릿: JdbcTemplate, NamedParameterTemplate

**JdbcTemplate 클래스 제공 메서드**

1. queryForObject: 하나의 결과 레코드 중 하나의 컬럼 값을 가져올 때 사용
2. queryForMap: 하나의 결과 레코드 정보를 Map 형태로 매핑할 수 있음
3. queryForList: 여러개의 Map 형태의 결과 레코드를 다룰 수 있음
4. query: 여러개의 레코드를 객체로 변환하여 처리
5. Update: 데이터의 변경을 실행할 경우 사용

**탬플릿 클래스의 생성과 인젝션**

탬플릿 클래스를 XML 파일에 Bean으로 정의

```xml
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
    <property name="driverClassName" value="com.mysql.cj.jdbc.Driver" />
    <property name="url" value="jdbc:mysql://localhost:3306/springdb?allowPublicKeyRetrieval=true&amp;useUnicode=true&amp;characterEncoding=utf8&amp;useSSL=false&amp;serverTimezone=UTC" />
    <property name="username" value="root" />
    <property name="password" value="1234" />
    <property name="maxActive" value="5" />
</bean>

<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <constructor-arg ref="dataSource" />
</bean>

<bean class="org.springframework.jdbc.core.namedparam.NamedParameterJdbcTemplate">
    <constructor-arg ref="dataSource" />
</bean>
```