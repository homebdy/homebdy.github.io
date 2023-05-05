---
layout: post
title:  "MyBatis"
author: yj
category: [ Spring🌱 ]
tags: [ JAVA, SPRING ]
---


## <a href="#">MyBatis</a>

SQL과 자바 객체를 매핑하는 사상에서 개발된 데이터 베이스 접근용 프레임워크
- SQL을 체계적으로 관리할 수 있도록 함
- 자바 객체와 SQL 입출력 값의 바인딩

**주요 컴포넌트**

1. MyBatis 라이브러리
    - MyBatis 설정 파일
    - SqlSessionFactoryBuilder : MyBatis 설정 파일을 바탕으로 SqlSessionFactory 생성
    - SqlSessionFactory : sqlSession 생성을 위한 컴포넌트
    - SqlSession : SQL 발행과 트랜잭션 관리
    - Mapper 인터페이스 : 매핑 파일과 SQL에 대응하는 자바 인터페이스
    - 매핑 파일 : SQL과 OR 매핑을 XML에 파일 기술

2. MyBatis-Spring 라이브러리: 스프링 프레임워크와 연동
    - org.mybatis.spring.SqlSessionTemplate : sqlSession 인터페이스를 구현

**필요 라이브러리**

- spring-jdbc : 스프링이 제공하는 JDBC 래핑 모듈
- MyBatis-Spring : 마이바티스가 제공하는 프레임워크 간의 연동 라이브러리
- MyBatis : 마이바티스 프레임워크 모듈
- 커넥션 풀 지원 라이브러리 : commons-dbcp
- 사용할 데이터베이스 JDBC 라이브러리 : mysql-connector-java

**연동 설정 - 의존 관계 설정**

1. 커넥션 풀을 지원하는 데이터 소스 빈 등록
2. 스프링의 트랜잭션 관리자의 빈 등록
3. MyBatis의 SqlSessopmFactory 빈 등록
3. MyBatis-Spring의 SqlSessionTemplate 빈 등록

### MyBatis 연동 설정

1. 빈 등록 빛 의존 관계 설정

```
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
    <property name="driverClassName" value="com.mysql.cj.jdbc.Driver" />
    <property name="url" value="jdbc:mysql://127.0.0.1:3306/springdb?
    allowPublicKeyRetrieval=true&amp;useUnicode=true&amp;characterEncoding=utf8&amp;
    useSSL=false&amp;serverTimezone=UTC" />
    <property name="username" value="spring" />
    <property name="password" value="passwd" />
    <property name="maxActive" value="5" />
</bean>
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"></property>
</bean>
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"></property>
    <property name="configLocation" value="classpath:/mybatis-config.xml"></property>
    <property name="mapperLocations" value="classpath:mappers/*Mapper.xml"></property>
</bean>
<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate" destroy-method="clearCache">
    <constructor-arg name="sqlSessionFactory" ref="sqlSessionFactory"></constructor-arg>
</bean>
```

2. mapper.xml 생성

```
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"></property>
    <property name="configLocation" value="classpath:/mybatis-config.xml"></property>
    <property name="mapperLocations" value="classpath:mappers/*Mapper.xml"></property>
</bean>
```

3. mybatis-config.xml 설정

- MyBatis의 공통적인 설정을 지정하는 XML 파일

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <typeAliases>
        <package name="org.tukorea.myweb.domain" />
    </typeAliases>
</configuration>

```