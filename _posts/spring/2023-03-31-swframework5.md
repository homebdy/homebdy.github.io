---
layout: post
title:  "DI - JAVA"
author: yj
category: [ Spring🌱 ]
tags: [ JAVA, SPRING ]
---

## <a href="#">JAVA 방식의 의존 관계 주입</a>

XML 문법 대신 자바 코드로 빈을 설정한다.

- 장점: 리팩토링에 적합 - 클래스 명이 틀린 경우 컴파일 에러 발생
- 컨테이너 생성 클래스: AnnotationConfigApplicationContext

**주요 Annotation**

자바 코드를 이용해 빈 객체를 생성하고 빈 객체 간의 의존 관계 설정

- `@Configuration`: 빈 설정 메타 정보를 담고 있는 클래스 선언
- `@Bean`: 클래스 내의 메서드 정의하여 새로운 빈 객체를 정의할 때 사용

**자바 설정과 XML 관계**

- 자바 설정에서는
    - @Bean 애노테이션과 메서드 이름을 이용해 컨테이너가 사용할 빈 객체를 생성
    - @Bean 매서드를 불러들여 객체를 취득
- XML에서는 \<property>, \<constructor-arg> 태그를 이용해 설정
- 자바에서는 빈 객체를 직접 생성한다.

### JAVA 설정을 통한 DI 예제 코드

1. JavaConfig.java 설정

```java
package org.tukorea.di.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.tukorea.di.persistence.MemberDAO;
import org.tukorea.di.persistence.MemberDAOImpl;
import org.tukorea.di.service.MemberService;
import org.tukorea.di.service.MemberServiceImpl;

@Configuration
public class JavaConfig {
	
	@Bean
	public MemberDAO memberDAO() {
		return new MemberDAOImpl();
	}
	
	@Bean(name="Service")
	public MemberService memberService() {
		return new MemberServiceImpl(memberDAO());
	}
}
```

2. MemberSampleMain.java

```java
package org.tukorea.di.main;import org.tukorea.di.config.JavaConfig;
import org.tukorea.di.domain.StudentVO;
import org.tukorea.di.service.MemberService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class MemberSampleMain {
	
	private static ApplicationContext ctx = null;
	
	public static void main(String[] args) throws Exception {
		System.out.println("안녕하세요 DI-Java!");
		
		ctx = new AnnotationConfigApplicationContext(JavaConfig.class);
		MemberService memberService = (MemberService)ctx.getBean(MemberService.class);
		
		StudentVO vo = new StudentVO();
		vo.setId("kanadara");
		memberService.addMember(vo);
		
		StudentVO member = memberService.readMember("kanadara");
		System.out.println(member);
	}
}
```

- DAO, VO, Service 코드는 Anootation 방식과 동일