---
layout: post
title:  "스프링 MVC"
author: yj
category: [ Spring🌱 ]
tags: [ JAVA, SPRING, MVC ]
---

## <a href="#">Spring MVC</a>

**특징**

1. 프론트 컨트롤러 패턴에 기초한 MVC 프레임워크
2. 모델, 뷰, 컨트롤러의 인터페이스가 정의되어 있어 구현에 의존적이지 않다.
    - 약한 결합도로 구성되어 유연하고 확장하기 쉬움
3. 다양한 서드 파티 라이브러리 연계를 지원
    - Jackson, Google Gson 등
4. 애노테이션 도입으로 스프링 MVC 보급이 확대
    - @Controller, @RequestMapping 등의 선언으로 간단하게 활용 가능

### 자바 웹 어플리케이션 설계 방식

**1. Model1 방식**

JSP만 사용하여 개발하거나 Java bean을 포함하여 개발하는 방식
- JSP에 뷰와 비즈니스 로직이 혼재되어 복잡도가 높고 유지보수하기 어렵다.

**2. Model2 방식**

Model-View-Controller로 분리
- 뷰와 비즈니스 로직의 분리로 유지보수하기 쉽고 확장에 용의하다.
- Controller: 데이터의 처리와 화면의 분기를 담당
- View: 화면상의 처리
- Model - 뷰에 필요한 비즈니스 데이터

**3. 프론트 컨트롤러 패턴**

클라이언트의 요청을 별도의 프론트 컨트롤러에 집중시키는 방식
- 모든 요청의 공통 부분을 별도의 프론트 컨트롤러로 처리한다.
- 이 프론트 컨트롤러를 `DispatcherServlet`이라고 함
- 실행 프로세스
    1. DispatcherServlet이 HTTP 요청을 받는다.
    2. DispatcherServlet은 서브 컨트롤러에 요청을 위임한다.
    3. 서브 컨트롤러에서 클라이언트 요청 처리를 위해 DAO를 호출
    4. DAO 객체는 리소스를 엑세스해 Model 객체를 생성하고 요청 결과를 리턴한다.
    5. DispatcherServlet은 처리 결과에 적합한 뷰에 화면 처리를 요청한다.
    6. 선택된 뷰는 화면에 모델 객체를 가져와 화면을 처리한다.
    7. HTTP 응답

- 구성요소
    1. DispatcherServlet: 프론트 컨트롤러로 클라이언트의 요청을 받아 서브 컨트롤러에게 요청을 전달하고 리턴 결과를 뷰에 전달해 응답을 생성한다.
    2. HandlerMapping: URL과 요청 정보를 기준으로 어떤 컨트롤러를 실행할지 결정하는 객체
        - 애노테이션 방식으로 이용할 경우 mvn:annotation-driven 태그 설정 필요
        - DispatcherServlet은 하나 이상의 핸들러 매핑을 가질 수 있다.
    3. Controller: 클라이언트의 요청을 처리하고 그 결과를 DispatcherServlet에 전달한다.
    4. Model: 컨트롤러가 뷰에게 넘겨줄 데이터를 저장하기 위한 객체
    5. ViewResolver: Controller가 리턴한 뷰 이름을 기반으로 Controller 처리 결과를 생성할 뷰를 결정한다.
    6. View: Controller의 처리 결과 화면을 생성한다.

### 스프링 스테레오타입 애노테이션

- @Component, @Service, @Controller, @Repository는 `스프링이 관리하는 컴포넌트`를 나타내는 일반적인 스테레오 타입
- ervice, Presentation, Persistence 계층의 컴포넌트는 @Service, @Controller, @Repository 스테레오 타입을 사용하여 구체화
- @Service, @Controller, @Repository는 @Component의 전문화된 타입

**애노테이션 종류**

1. @Component: 일반적인 컴포넌트
2. @Repository: Persistence계층 컴포넌트
3. @Service: Business계층 컴포넌트
4. @Controller: Presentation 계층 컴포넌트
5. @RestController: @Controller + @ResponseBody

## <a href="#">MVC 환경 설정</a>

**기본 설정**

- spring-webmvc를 설정하면 스프링 웹과 기타 스프링 프레임워크의 의존 모듈에 대한 의존 관계도 함께 처리 된다.
- 스프링 MVC에서는 Bean Validation 구조체(hibernate-validator)를 이용해 자바 빈 값의 유효성을 애노테이션을 통해 검증한다.

**웹 애플리케이션 컨텍스트 등록 설정**

웹 애플리케이션에 사용할 2가지 애플리케이션 컨텍스트를 등록한다.

1. ContextLoadListner: 서비스 계층 이하의 빈(@Service, @Repository 등)을 등록하기 위한 클래스
2. DispatcherServlet: 컨트롤러 빈을 등록하기 위한 클래스
    - 서블릿 컨테이너에 프론트 컨트롤러인 DispatcherServlet 클래스 등록

- web.xml

    ```    
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/spring/root-context.xml</param-value>
    </context-param>

    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listenerclass>
    </listener>

    <servlet>
        <servlet-name>appServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>appServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    ```


**컨트롤러 구조**

모든 컨트롤러에는 `@Controller`애노테이션을 설정
- 메스드 별로는 `@RequestMapping` 애노테이션을 통해 URL을 매핑한다.
- `@RequestParam`은 Http 요청 파라미터를 메소드의 파라미터로 전달받을 때 사용
- 설정 방식
    1. DispatcherServlet 클래스의 설정 파일인 servlet-context.xml에서 컨트롤러를 등록한다.
    2. \<annotation-driven />: 패키지 내부에서 찾은 빈과 URI를 매핑한다.
    2. \<context:component-scan base-package="org.tukorea.web.controller" />
        - base-package 내부의 클래스에서 @Controller 지정된 컨트롤러를 검색하여 빈으로 등록
- DispatcherServlet 설정 코드

    ```
    <annotation-driven />

    <resources mapping="/resources/**" location="/resources/" />

    <beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <beans:property name="prefix" value="/WEB-INF/views/" />
        <beans:property name="suffix" value=".jsp" />
    </beans:bean>

    <context:component-scan base-package="org.tukorea.web.controller" />
    ```

- ContextLoadListner 설정: @Service로 지정된 클래스를 빈으로 등록한다.

    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context-4.3.xsd">

        <bean id="validator" class="org.springframework.validation.beanvalidation.LocalValidatorFactoryBean">
        </bean>
        <context:component-scan base-package="org.tukorea.web.service" />
    </beans>
    ```

### @RequestMapping

URL과 컨트롤러 메서드의 매핑을 설정하는 애노테이션
- 속성값을 통해 URL을 설정한다.
- 속성
    1. value: url 표시
    2. path: value의 별명
    3. method: HTTP 메서드
    4. params: 요청 파라미터 유무나 파라미터 값

**매핑 설정 방식**

1. @PathVariable 적용 변수로 전달
    - `@RequestMapping(value="/try/{msg}", method = RequestMethod.GET)`

2. @RequestParam 적용 변수로 전달

```
@RequestMapping(value="/tryA", method = RequestMethod.GET)
public String getUserTest1( @RequestParam("msg") String msg)
```

3. @ModelAttribute 변수로 전달

```
@RequestMapping(value="/tryB", method = RequestMethod.GET)
public String getUserTest2( @ModelAttribute("msg") String msg) 
```

4. @RequestMapping(value={"/tryC", "/tryD"})
    - 배열의 형태로 지정 가능