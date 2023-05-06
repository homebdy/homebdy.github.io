---
layout: post
title:  "스프링 테스트"
author: yj
category: [ Spring🌱 ]
tags: [ JAVA, SPRING ]
---

## <a href="#">Spring Test</a>

스프링 빈을 테스트하는 모듈
- JUnit 테스팅 프레임워크를 사용하여 스프링의 DI 컨테이너를 동작시키는 기능

**주요 애노테이션**

1. `@RunWith`: 스프링 테스트를 위한 DI 컨테이너를 로딩
2. `@ContextConfiguration`: DI 컨테이너의 설정 파일 위치 또는 설정 클래스 지정

**스프링 TestContext 프레임워크**

스프링 테스트에서 동작하는 테스트용 프레임 워크
- JUnit에서 스프링 TestContext 프레임워크를 동작시킬 수 있도록 지원하기 위해 SpringJUnit4ClassRunner 제공

**Test 1. 데이터 소스 연결 작동 여부 테스트**

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = {“file:src/main/webapp/WEB-INF/spring/root-context.xml“ })
public class DataSourceTest {

    @Inject
    private DataSource ds;

    @Test
    public void testConntection() throws Exception {
        try(Connection con = ds.getConnection()) {
            System.out.println(con);
        }
        catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

**Test 2. sqlSession 객체 생성 여부 테스트**

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = { "file:src/main/webapp/WEB-INF/spring/root-context.xml" })
public class DataSourceTest {

    @Inject
    private SqlSessionFactory sqlFactory;

    @Test
    public void testFactory() throws Exception {
        try(SqlSession session = sqlFactory.openSession()) {
            System.out.println(session);
        }
        catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```