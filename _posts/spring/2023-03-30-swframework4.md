---
layout: post
title:  "DI - Annotation"
author: yj
category: [ Spring🌱 ]
tags: [ JAVA, SPRING ]
---

## <a href="#">Annotation 방식의 의존 관계 주입</a>

메타데이터를 XML등의 문서에 설정하는 것이 아니라 소스코드에 `@Annotation`의 형태로 표현
- 클래스, 메소드, 필드의 선언부에 표현하여 특정 기능이 적용되었음을 명시
- 애플리케이션 규모가 커질수록 XML 설정이 복잡해져 Annotation 방식을 적용하여 개선

### 주요 Annotation

1. `@Autowired`: 컨테이너가 빈과 다른 빈과의 의존성을 자동으로 연결하도록 하는 수단
- 인스턴스 변수 앞에 Annotation을 붙이면 해당 타입의 Component를 찾아 그 인스턴스를 주입시켜준다
-  `<context:annotation-config/>` 설정이 필요하다.

2. `@Component`: 컨테이너가 인젝션을 위한 인스턴스를 설정하는 수단
- 클래스 선언 앞에 Annotation을 붙이면 컨테이너가 찾아서 관리하고 @Autowired가 붙어있는 인스턴스 변수에 주입시킨다.
- `<context:component-scan base-package=“패키지 이름"/>` 선언 후 사용할 수 있다.

#### <context:component-scan base-package=“패키지 이름"/> 와 \<context:annotation-config/>의 차이

**\<context:annotation-config />**
-  @Autowired, @Resource, @Required 어노테이션을 이용할 때 사용
- XML에 bean을 선언하여 등록한 후 등록된 빈들의 Annotation 기능을 적용하기 위해 사용된다. 
- 단, `<context:component-scan />`가 이미 설정되어 있는 경우 생략할 수 있다.

**\<context:component-scan base-package=“명”/>**
- @Component, @Service, @Repository, @Controller 사용 시 선언 필요
- bean의 등록 여부와 관계 없이 스프링이 base-package인 ‘명’ 패키지를 스캔하여 등이 선언되어 있는 모든 클래스를 스캔한 후 빈을 DI 컨테이너에 등록하는 설정

#### @Component 확장 애노테이션

1. @Controller: 프레젠테이션 층 스프링 MVC용 애노테이션
2. @Repository: 데이터 액세스 층의 DAO용 애노테이션
3. @Service: 비즈니스 로직층 Service용 애노테이션
4. @Configuration: Bean 정의를 자바 프로그램에서 실행하는 JavaConfig용 애노테이션

### Annotation을 통한 DI 예제 코드

1. ApplicationContext.xml 설정

	```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xmlns:c="http://www.springframework.org/schema/c"
		xmlns:context="http://www.springframework.org/schema/context"
		xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
			http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd">

		<context:component-scan base-package="group.iddi.persistence"></context:component-scan>
		<context:component-scan base-package="group.iddi.service"></context:component-scan>
	</beans>
	```

2. MemberDAOImpl.java 설정

	``` java
	package group.iddi.persistence;
	import java.util.HashMap;
	import java.util.Map;

	import org.springframework.stereotype.Component;
	import group.iddi.domain.StudentVO;

	@Component
	public class MemberDAOImpl implements MemberDAO {
		
		private Map<String, StudentVO> storage = new HashMap<String, StudentVO> ();

		public StudentVO read(String id) throws Exception {
			// TODO Auto-generated method stub
			return storage.get(id);
		}

		public void add(StudentVO student) throws Exception {
			// TODO Auto-generated method stub
			storage.put(student.getId(), student);
		}
	}
	```

3. MemberServiceImpl.java 설정

	``` java
	package group.iddi.service;
	import org.springframework.beans.factory.annotation.Autowired;
	import org.springframework.stereotype.Component;
	import group.iddi.domain.*;
	import group.iddi.persistence.*;

	@Component
	public class MemberServiceImpl implements MemberService {
		
		@Autowired
		private MemberDAO memberDAO;
		
		public MemberServiceImpl(MemberDAO memberDAO) {
			this.memberDAO = memberDAO;
		}
		
		public StudentVO readMember(String userId) throws Exception {
			return memberDAO.read(userId);
		}

		public void addMember(StudentVO student) throws Exception {
			memberDAO.add(student);
		}
	}

	```

4. Main 실행

	``` java
	package group.iddi.main;
	import group.iddi.domain.StudentVO;
	import group.iddi.service.MemberService;
	import org.springframework.context.ApplicationContext;
	import org.springframework.context.support.GenericXmlApplicationContext;

	public class MemberSampleMain {
		
		private static ApplicationContext ctx = null;
		public static void main(String[] args) throws Exception {
			System.out.println("안녕하세요 DI-XML");
			
			ctx = new GenericXmlApplicationContext("classpath:applicationContext.xml");
			MemberService memberService = (MemberService)ctx.getBean(MemberService.class);
			
			StudentVO vo = new StudentVO();
			vo.setId("kanadara");
			memberService.addMember(vo);
			
			StudentVO member = memberService.readMember("kanadara");
			System.out.println(member);
		}
	}
	```
