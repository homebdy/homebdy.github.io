---
layout: post
title:  "스프링 빈 설정 메타 정보"
author: yj
category: [ Spring🌱 ]
tags:  [ JAVA, SPRING ]
---

### <a href="#">BeanDefinition: 스프링 빈 설정 메타 정보</a>
- 스프링의 다양한 설정 형시 지원의 중심에는 `BeanDefinition`이라는 추상화가 존재
- 역할과 구현을 개념적으로 나눈 것
    > XML을 읽어 BeanDefinition을 만듦
    > 자바 코드를 읽어 BeanDefinition을 만듦
    > 스프링 컨테이너는 오직 BeanDefinition만 알면 된다.
- `BeanDefinition`을 설정 메타정보라고 한다.
    - `@Bean`, `<bean>` 당 각각 하나씩 메타 정보 생성
- 스프링 컨테이너는 이 메타정보를 기반으로 스프링 빈 생성
- 생성 과정
    1. `AnnotationConfigApplicationContext`는 `AnnotatedBeanDefinitionReader`를 사용하여
        빈 생성 파일을 읽고 `BeanDefinition`을 생성
    2. `GenericXmlApplicationContext`는 `XmlVeanDefinitionReader`를 사용해 ~.xml 설정 정보를 읽고 `BeanDefinition`을 생성
    3. 새로운 형식의 설정 정보 추가 시 xxBeanDefinitionReader를 만들어 `BeanDefinition`을 생성한다.

    ##### [출처]
- [인프런 스프링 핵심 원리 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)