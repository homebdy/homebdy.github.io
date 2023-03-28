---
layout: post
title:  "DI - XML"
author: yj
category: [ Spring🌱 ]
tags: [ JAVA, SPRING ]
---

## <a href="#">XML 방식의 의존 관계 주입</a>

XML 파일에서 `<Bean>` 요소를 정의하여 설정한다.

**예제 프로젝트 구성**

- StudentVO: 회원 정보 도메인 클래스
- MemberSampleMain: 애플리케이션 메인 객체
- MemberDAO: 회원정보 DAO 인터페이스 
- MemberDAOImpl: MemberDAO 인터페이스 구현 객체
- MemberService: 회원 정보 서비스 인터페이스
- MemberServiceImpl: 회원 정보 서비스 인터페이스 구현
- applicationContext.xml: 스프링 설정 파일

인터페이스와 컨테이너를 이용해 클래스 구현
- 변경 또는 확장이 되더라도 해당 클래스를 이용하는 다른 클래스에 영향 범위를 최소화 할 수 있음
- DI를 이용한 `인터페이스 기반의 컴포넌트화` 라고 한다..

**과정**

1. Pom.xml 설정

	```
	<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
		<modelVersion>4.0.0</modelVersion>
		<groupId>그룹 아이디</groupId>
		<artifactId>di-xml</artifactId>
		<version>0.0.1-SNAPSHOT</version>
		<properties>
			<org.springframework.version>>5.2.22.RELEASE</org.springframework.version>
			<slf4j.version>1.7.36</slf4j.version>
			<logback.version>1.2.11</logback.version>
		</properties>
		<dependencies>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-context</artifactId>
				<version>5.2.22.RELEASE</version>
			</dependency>
			<dependency>
				<groupId>org.slf4j</groupId>
				<artifactId>slf4j-api</artifactId>
				<version>${slf4j.version}</version>
				<scope>compile</scope>
			</dependency>
			<dependency>
				<groupId>ch.qos.logback</groupId>
				<artifactId>logback-classic</artifactId>
				<version>${logback.version}</version>
				<scope>runtime</scope>
			</dependency>
		</dependencies>
	</project>
	```

2. applicationContext.xml 네임스페이스 추가
    - beans: 빈 설정
    - c: 생성자 인자를 \<bean> 요소 어트리뷰트로 선언
    - context: 빈 검색과 어노테이션 설정

3. StudentVO.java: src/main/java/group/id/domain

	`VO`: 값 오브젝트로써 사용
	- readOnly 특성

	```python
	public class StudentVO {
		
		private String id;
		private String passwd;
		private String username;
		private String snum;
		private String depart;
		private String mobile;
		private String email;
		
		public String getId() {
			return id;
		}
		
		public String getpasswd() {
			return passwd;
		}

		public String getUsername() {
			return username;
		}

		public String getSnum() {
			return snum;
		}

		public String getPasswd() {
			return passwd;
		}

		public void setPasswd(String passwd) {
			this.passwd = passwd;
		}

		public void setId(String id) {
			this.id = id;
		}

		public void setUsername(String username) {
			this.username = username;
		}

		public void setSnum(String snum) {
			this.snum = snum;
		}

		public void setDepart(String depart) {
			this.depart = depart;
		}

		public void setMobile(String mobile) {
			this.mobile = mobile;
		}

		public void setEmail(String email) {
			this.email = email;
		}

		public String getDepart() {
			return depart;
		}

		public String getMobile() {
			return mobile;
		}

		public String getEmail() {
			return email;
		}
		
		@Override
		public String toString() {
			return "StudentVO [id=" + id + ", passwd=" + passwd + ", username=" + username + ", snum=" + snum + ", depart="
					+ depart + ", mobile=" + mobile + ", email=" + email + "]";
		}
	}
	```

4. MemberDAO: group.id.di.persistence

	```python
	package group.id.di.persistence;
	import group.id.di.domain.StudentVO;

	public interface MemberDAO {
		
		public StudentVO read(String id) throws Exception;
		public void add(StudentVO student) throws Exception;
	}

	```

5. MemberDAOImpl: group.id.di.persistence

	```python
	package group.id.di.persistence;
	import java.util.HashMap;
	import java.util.Map;
	import group.id.di.domain.StudentVO;

	public class MemberDAOImpl implements MemberDAO {
		
		private Map<String, StudentVO> storage = new HashMap<String, StudentVO> ();

		@Override
		public StudentVO read(String id) throws Exception {
			// TODO Auto-generated method stub
			return storage.get(id);
		}

		@Override
		public void add(StudentVO student) throws Exception {
			// TODO Auto-generated method stub
			storage.put(student.getId(), student);
		}
	}

	```

6. MemberService.java: group.id.di.service
	```
	import group.id.di.domain.StudentVO;

	public interface MemberService {

		public StudentVO readMember(String userId) throws Exception;
		public void addMember(StudentVO student) throws Exception;
	}
	```

7. MemberServiceImpl.java: group.id.di.service

	```python
	import group.id.di.domain.*;
	import group.id.di.persistence.*;

	public class MemberServiceImpl implements MemberService {

		private MemberDAO memberDAO;
		
		public MemberServiceImpl(MemberDAO memberDAO) {
			this.memberDAO = memberDAO;
		}
		
	//	public void setMemberDAO( MemberDAO memberDAO ) {
	//		this.memberDAO = memberDAO;
	//	}

		@Override
		public StudentVO readMember(String userId) throws Exception {
			return memberDAO.read(userId);
		}

		@Override
		public void addMember(StudentVO student) throws Exception {
			memberDAO.add(student);
		}
	}
	```

8. MemberSampleMain.java

	```python
	package group.id.di.main;
	import group.id.di.domain.StudentVO;
	import group.id.di.service.MemberService;
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

#### 의존성 주입 방식

**1. 생성자 기반 의존성 주입**

생성자의 인수를 사용해 의존성을 주입한다
- XML 설정 파일에 \<constructor-arg>태그를 사용해 주입할 컴포넌트 설정
- 예시

```
<bean id="memberService" class="group.id.di.service.MemberServiceImpl">          
    <constructor-arg ref="memberDAO"/>
</bean>
```

**2. 설정자 기반 의존성 주입**

설정자의 인수를 사용해 의존성을 주입한다
- XML 설정 파일에 \<property>태그의 name 속성에 주입할 설정
- 예시

```
<bean id="memberService" class="group.id.di.service.MemberServiceImpl"> 
    <property name = "memberDAO" ref="memberDAO" />
</bean>
```


##### bean 태그의 속성

1. id: 오브젝트를 유일하게 하는 ID
2. name: 오브젝트의 이름 정의
3. class: id의 실체, 패키지명과 클래스 명으로 구성
4. scope: 오브젝트의 스코프를 지정 - default = singleton

등등 다양한 속성이 존재한다.