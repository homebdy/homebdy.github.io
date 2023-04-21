---
layout: post
title:  "스프링 JDBC - 제공 메서드"
author: yj
category: [ Spring🌱 ]
tags: [ JAVA, SPRING ]
---


## <a href="#">스프링 JdbcTemplate 클래스 제공 메서드</a>

1. queryForObject: 하나의 결과 레코드 중 하나의 컬럼 값을 가져올 때 사용
2. queryForMap: 하나의 결과 레코드 정보를 Map 형태로 매핑할 수 있음
3. queryForList: 여러개의 Map 형태의 결과 레코드를 다룰 수 있음
4. query: 여러개의 레코드를 객체로 변환하여 처리
5. Update: 데이터의 변경을 실행할 경우 사용

#### SELECT문

**취득 결과가 레코드 건수 또는 특정 컬럼만 취득할 경우**

1. `queryForObject`

    사용 방식 1
    - 제 1인수: SQL 문자열
    - 제 2인수: 반환형 클래스 오브첵트
    ```
    JdbcTemplate jdbcTemplate = ctx.getBean(JdbcTemplate.class);
    int count = jdbcTemplate.queryForObject("SELECT COUNT(*) FROM STUDENT", Integer.class);
    ```

    사용 방식 2
    - 제 1인수: SQL 문자열
    - 제2인수 : 반환형 클래스 오브젝트(String)
    - 제 3인수 : 파라미터 값
    ```
    String name = jdbcTemplate.queryForObject("SELECT username FROM STUDENT WHERE id= ?", String.class, id);
    ```

**취득 결과가 한 레코드 값을 취득할 경우**

1. `queryForMap`

    레코드 값을 Map데이터로
    ```
    Map<String, Object> student = jdbcTemplate.queryForMap("SELECT * FROM STUDENT WHERE id= ?", id);
    String name = (String)student.get("username"); 
    ```

2. `queryForList`

    여러 레코드 값을 Map데이터로
    ```
    List<Map<String, Object>> studentList = jdbcTemplate.queryForMap("SELECT * FROM STUDENT ");
    ```

**도메인으로 변환할 경우**

1. `queryForObject`
    하나의 레코드를 가져오는 경우
    - 제 1인수: SELECT 문
    - 제 2인수: 도메인 자동 변환을 위한 스프링 제공 클래스 `BeanPropertyRowMapper`
    - 제 3인수: SELECT 문의 파라미터
    - BeanPropertyRowMapper를 사용할 경우 VO의 프로퍼티 명과 테이블 컬럼 명이 같아야 한다.

2. `query`
    여러 레코드를 가져오는 경우
    - RowMapper 인터페이스를 구현한 익명 클래스를 정의
    - 클래스내 mapRow() 추상 메서드를 정의

#### INSERT / UPDATE / DELETE 문

모두 update 메서드를 사용해 구현

1. INSERT

```
StudentVO vo;
jdbcTemplate.update(
    "INSERT INTO STUDENT (ID, PASSWD, USERNAME, SNUM, DEPART, MOBILE, EMAIL) VALUES (?, ?, ?, ?, ?, ?, ?) ", 
    vo.getId(), vo.getPasswd(), vo.getUsername(), vo.getSnum(), vo.getDepart(), vo.getMobile(), vo.getEmail()
);
```

2. DELETE

```
StudentVO vo;
jdbcTemplate.update( “DELETE FROM STUDENT WHERE ID=? “, vo.getId());
```