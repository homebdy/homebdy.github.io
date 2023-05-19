---
layout: post
title:  "스프링 부트"
author: yj
category: [ Spring🌱 ]
tags: [ JAVA, SPRING ]
---


## <a href="#">Spring boot</a>

독립적으로 실행되는 스프링 기반 애플리케이션을 쉽게 만들 수 있게 하는 프로젝트

**특징**

1. starter: 의존 관계 라이브러리를 간단하게 정의하는 모듈 제공
    - 미리 정의한 의존 관계 라이브러리들을 묶어 제공    
    - spring-boot-starter-web : 하나의 의존 관계를 추가
    - spring-boot-starter-parent : 검증된 의존 관계 버전정보 조합을 제공
2. 구성 클래스: 의존 라이브러리 설정은 애너테이션과 자바 방식으로 설정
3. 자동 구성: 프로젝트 관련 디폴트 구성이 적용되며 필요한 부분만 설정
4. 메인 애플리케이션 클래스: 자바 명령으로 내장된 톰캣 실행
    - war파일을 서버에 배포하지 않고 서버 애플리케이션을 만들 수 있음
5. 설정 파일: 속성을 위부 파일에 정의할 수 있어 쉽게 변경할 수 있다.
    - application.properties, application.yml

**스프링과 스프링 부트의 차이**

- 스프링
    1. 확장자: war
    2. sprig-web
    3. 서버 설정: server.xml에 설정
    4. 기본적인 라이브러리 설정과 버전 관리를 수동으로 관리

- 스프링 부트
    1. 확장자: jar
    2. spring-boot-starter-web
    3. 별도의 톰캣, web.xml 설정을 하지 않는다.
    4. 서버 설정: application.properties, application.yml에 설정
    5. starter 패키지를 이용하면 기본적인 라이브러리에 대한 의존성을 제공하여 설정을 최소화

### @SpringBootApplication

스프링부트 프로젝트 생성 시 메인 클래스에 자동으로 생성하는 애노테이션
- @SpringBootConfigration, @ComponentScan, @EnableAutoConfiguration 병합
- @SpringBootConfigration: Configuration
- @ComponentScan: 명시한 클래스들을 찾아 스프링 빈으로 등록해주는 애노테이션
- @EnableAutoConfiguration: 사전에 정의한 라이브러리들을 대상으로 조건이 만족될 경우 빈으로 등록해주는 애노테이션

### 프로젝트 생성 방법

1. [Spring initializr](https://start.spring.io/) 접속
2. 필요 설정, Dependencies 추가 후 Generate

<img src="https://cdn.discordapp.com/attachments/1065625657662509200/1109121853475278999/image.png" width="600" height="800"/>