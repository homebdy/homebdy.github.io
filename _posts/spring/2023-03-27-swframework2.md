---
layout: post
title:  "Dependency Injection"
author: yj
category: [ Spring🌱 ]
tags: [ JAVA, SPRING ]
---

엔터프라이즈 애플리케이션 개발 시 하나의 기능을 처리하기 위해 여러 컴포넌트를 조합해 구현하는 경우가 일반적
- 컴포넌트 종류: DB 컴포넌트, GUI 관련 컴포넌트, 외부 접속 컴포넌트 등...

이러한 여러개의 컴포넌트를 통합할 때 `의존 관계 주입(Dependency Injection)` 디자인 패턴이 효과적

## <a href="#">의존 관계 주입 DI</a>

오브젝트 간의 의존 관계를 만드는 것
- 스프링 프레임워크에서는 `런타임시` 사용할 객체들의 의존 관계를 부여한다.
- 객체 간의 결합도를 낮추는 방식

### IoC

인스턴스를 제어하는 주도권이 역전된다는 의미
- 컴포넌트를 구성하는 인스턴스 생성과 의존 관계 연결을 개발자의 소스코드가 아닌 DI 컨테이너가 대신 해주는 것

- IoC 컨테이너(=DI 컨테이너): 인스턴스의 생명주기 관리 및 의존 관계 주입을 처리하는 컨테이너

**일반적인 애플리케이션에서의 의존 관리**

`new`연산자 이용
- main 함수에서 Service 셍상 후 Service에서 DAO 생성
- 예시
``` java
Class MemberService {
    // memberService에서 memberDAO 생성
    MemberDAO memberDAO = new MemberDAO(); 
}
```

**DI 컨테이너를 활용한 애플리케이션**

DI 컨테이너에서 MemberMain이 이용하는 Service 인스턴스와 Service에서 이용하는 Dao 인스턴스를 생성
- Dao 인스턴스를 Service에 주입: `의존 관계 주입`
- `인터페이스` 기반 컴포넌트 화
    - Service와 Dao를 인터페이스로 하고 구현 클래스는 인터페이스 이름에 Impl을 붙여 생성

### Spring Bean

스프링 컨테이너가 관리하는 객체

#### IoC 컨테이너

스프링 빈의 생성, 관계, 조립. 생명주기를 관리하는 스프링 프레임워크의 핵심
- 의존관계주입을 이용해 애플리케이션을 구성하는 컴포넌트들을 관리

##### 종류

**1. BeanFactory**

빈의 생성, 빈의 의존관계 관리 등의 DI 기본 기능 제공
- 빈이 많지 않고 경량 컨테이너로 작업할 경우 활용
- XML 파일로부터 설정 정보를 활용하는 가장 많이 사용되는 클래스

**2. ApplicationContext**

일반적인 스프링 컨테이너
- BeanFactory 인터페이스를 상속받은 하위 인터페이스로 확장된 기능 제공
- XML 파일로부터 설정 정보를 활용하는 가장 많이 사용되는 클래스

**WebApplicationContext**

웹 애플리케이션을 위한 ApplictiaonContext

- 종류
    1. ContextLoaderListener에 의해 생성되는 WAC
        - dao, service관련 스프링 빈들 등록
        - 웹 애플리케이션 전체에서 사용할 WAC 객체 생성
        - root-context.xml 파일에서 설정
    
    2. DispatcherServlet에 의해 설정되는 WAC
        - 컨트롤러오 ㅏ같은 서블릿 관련 빈 등록
        - 해당 서블릿마다 사용할 WAC 객체 생성
        - servlet-context.xml 파일에 설정

- 컨테이너 인스턴스화: web.xml에서 설정
    : ContextloaderListener, DispatcherServlet을 사용하여 ApplicationContext 생성


#### DI 설정 방법

1. XML 기반 설정: XML 파일을 사용하는 `<Bean>` 요소를 정의하는 방식
2. Annotation: `@Component` 에노테이션이 부여된 클래스를 DI 컨테이너가 Bean으로 자동 등록하는 방법
3. Java: 자바 클래스에 `@Configuration` 애노테이션을, 메서드에 `@Bean` 에노테이션을 사용해 빈을 등록하는 방법