---
layout: post
title:  "Spring Security"
author: yj
category: [ Spring🌱 ]
tags: [ JAVA, SPRING ]
---


## <a href="#">Spring Security</a>

애플리케이션 보안 기능을 구현할 때 사용하는 프레임워크

- 기본적인 보안기능
    1. 인증: 애플리케이션에 접근하는 사용자를 특정하는 기능
    2. 인가: 특정한 사용자에 대해 정보와 기능의 접근을 제한하는 기능

- 강화된 보안 기능
    1. 세션 관리
        - 세션 라이프 사이클 관리
    2. CSRF
        - 크로스 사이트 요청 변조 공격으로부터 사용자 보호
    3. 브라우저 보안기능 연계
        - 브라우저 기능을 악용한 공격에서 사용자 보호

**특징**

- 인증, 인가 구현을 위한 다양한 필터 클래스 제공
- XML 파일에서 데이터베이스 리소스로부터 인증, 인가 정보 취득 가능
- HTTP basic인증, HTML 폼 등 다양한 인증 방식 지원
- 인가 정보에 기반한 화면 제어를 위한 JSP 태그 라이브러리 제공
- 메서드 호출에 대한 접근 제어에 AOP 사용 가능
- spring-security-oauth2 라이브러리 제공
    - 하나의 아이디로 여러 사이트에 로그인 할 수 있는 체계
- Modernized Password Encoding
    - DelegatingPasswordEncoder 제공: bcrypt

### Spring Security 구조

**라이브러리 구성**
- spring-security-core: 인증 인가 기능을 구현하기 위한 핵심적인 컴포넌트
- spring-security-web: 웹 애플리케이션 보안 기능을 구현하기 위한 컴포넌트
- spring-securiy-config: 각 모듈에서 제공하는 컴포넌트 설정을 지원하기 위한 컴포넌트로 구성
- spring-securiyitaglibs: 인증 및 인가 정보를 사용하기 위한 JSP 태그 라이브러리로 구성
- spring-securiy-acl: ACL을 사용해 애플리케이션의 도메인 객체 인스턴스를 보호

**구조**

1. 요청
2. 필터보다 앞에 FilterChainProxy 처리 먼저 실시
3. 인증 방식에 맞게끔 FilterChain 실행

**주요 컴포넌트**

1. FilterChainProxy: 프레임 워크의 진입점 역할을 하는 서블릿 필터 클래스
- 전체 흐름을 제어하고 보안 기능과 같은 추가 기능을 필터에 위임하는 방식으로 동작

2. HttpFirewall: 방화벽 기능을 추가하기 위한 인터페이스
- 기본적으로 DefaultHttpFirewall 클래스 사용
- 인가되지 않은 요청을 차단하는 역할 수행

3. SecurityFilterChain
- FilterChainProxy가 받은 요청에 적용할 보안 필터 목록을 관리하기 위한 인터페이스
- 기본적으로 DefaultSecurityFilterChain 클래스 사용
- 요청 패턴별로 보안 필터 관리

4. Securiy Filter: 보안 기능을 제공하는 서블릿 필터 클래스


**스프링 시큐리티 인증 필터**

- 인증 처리 방식에 대한 구현을 제공하는 서블릿 필터
    - UsernamePasswordAuthenticationFilter(폼인증용 필터), basic 인증, Digest 인증, RememberMe 인증 등 다양한 서블릿 필터 클래스 제공
- 스프링 프레임워크의 웹 모듈에서는 서블릿 API의 인터페이스를 구현한 DelegationFilterProxy 클래스를 정의

**인증 처리 수행을 위한 인터페이스**

1. AuthenticationManager
- 인증 처리를 수행하기 위한 인터페이스
- 실제 인증 처리는 AuthenticationProvider에 위임하고 반환되는 처리 결과를 처리하는 구조로 동작

2. AuthenticationProvider: 인증 처리 기능을 구현하기 위한 인터페이스
- 스프링 시큐리티는 인증 방식별로 다양한 구현 클래스 제공