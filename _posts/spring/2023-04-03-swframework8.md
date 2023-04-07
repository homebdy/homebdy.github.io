---
layout: post
title:  "JUnit"
author: yj
category: [ Spring🌱 ]
tags: [ JAVA, SPRING ]
---

## <a href="#">스프링 테스트</a>

스프링 프레임워크에서 만든 클래스들을 테스트하는 모듈
- 단위 테스트, 통합 테스트를 지원하기 위한 매커니즘이나 편리한 기능을 제공
1. JUnit 테스트 프레임워크를 사용하여 스프링 DI 컨테이너 동작 
2. 트랜잭션 테스트를 상황에 맞게 제어
3. 애플리케이션 서버를 사용하지 않고 스프링 MVC 동작
4. RestTemplate을 이용해 HTTP 요청에 대한 임의 응답을 보냄

#### 스프링 테스트의 POM.xml 설정

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>${org.springframework-version}</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>${junit.version}</version>
    <scope>test</scope>
</dependency>
```

**테스트 케이스 작성**

```java
package org.tukorea.test;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.tukorea.di.domain.StudentVO;
import org.tukorea.di.service.MemberService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

@RunWith(SpringJUnit4ClassRunner.class) // 테스트용 DI 컨테이너를 동작시키기 위한 Runner Class
@ContextConfiguration(locations = "classpath:/applicationContext.xml")
public class MemberTest {

    @Autowired
    MemberService memberService; // DI 컨테이너에 등록할 테스트 대상 빈 주입

    @Test // 테스트 메서드 선언
    public void testReadMember( ) throws Exception {
        StudentVO member = memberService.readMember("hansol"); // 테스트 메서드 실행
        System.out.println(member);
    }
}
```

#### 테스트 시 순서 지정 방법 (JUnit4)

1. 필요 라이브러리 import
```java
import org.junit.runners.MethodSorters;
import org.junit.FixMethodOrder;
```

2. `@FixMethodOrder(MethodSorters.NAME_ASCENDING)` : 이름 오름차 순으로 테스트 실행

3. 테스트 이름 모듈 이름을 testA~, testB~ 작성