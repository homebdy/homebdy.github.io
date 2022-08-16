---
layout: post
title:  "컴포넌트 스캔"
author: yj
category:  Spring🌱
tags: [ JAVA, SPRING ]
---

### <a href="#">컴포넌트 스캔과 의존관계 자동 주입</a>
- Spring에서는 설정 정보가 앖더라도 스프링 빈을 자동으로 등록해주는 `컴포넌트 스캔`이라는 기능을 제공
- `@ComponentScan` 기능을 사용하여 컴포넌트 스캔 기능 이용 가능
- `@Component` 애노테이션이 붙은 클래스들을 스캔해 스프링 빈으로 자동 등록
- 의존 관계는 `@Autowired` 기능으로 자동 주입 가능

**ComponentScan 동작 방식**
1. `@ComponentScan`: 컴포넌트 스캔
- @Component가 붙은 모든 클래스를 스프링 빈으로 등록해줌

2. `@Autowired`: 의존관계 주입
- 생성자에 @Autowired를 지정하면 스프링 컨테이너가 자동으로 해당 스프링 빈을 찾아 의존관계 주입

### <a href="#">탐색 위치와 기본 스캔 대상</a>

- 모든 자바 클래스를 탐색하며 컴포넌트 스캔하면 시간이 오래걸림
→ 필요한 위치부터 탐색하도록 위치를 지정할 수 있다!
- `basePackages`: 탐색할 패키지 시작 위치 지정하고 하위 패키지 모두 탐색

**컴포넌트 기본 스캔 대상**
- `@Component`: 컴포넌트 스캔에서 사용
- `@Controller`: 스프링 MVC 컨트롤러로 인식. 스프링 MVC 컨트롤러에서 사용.
- `@Service`: 스프링 비즈니스 로직에서 사용.
- `@Repository`: 스프링 데이터 접근 계층에서 사용. 데이터 계층의 예외를 스프링 예외로 변환.
- `@Configuration`: 스프링 설정 정보에서 사용. 스프링 빈이 싱글톤을 유지하도록 추가 처리

### <a href="#">필터</a>
- `includeFilters`: 컴포넌트 스캔 대상 추가 지정
- `excludeFilters`: 컴포넌트 스캔에서 제외할 대상 지정

**FilterType 옵션**

1. ANNOTATION: 기본값, 에노테이션을 인식하여 동작
2. ASSIGNABLE_TYPE: 지정한 타입과 자식 타입을 인식하여 동작
3. ASPECTJ:AspectJ 패턴 사용
4. REGEX: 정규 표현식
5. CUSTOM: TypeFilter이라는 인터페이스를 구현하여 처리

### <a href="#">중복 등록과 충돌</a>
- 컴포넌트 스캔에서 같은 빈 이름을 등록하는 경우: 자동빈 vs 자동빈 / 수동빈 vs 자동빈<br/>

**1. 자동 빈 등록 vs 자동 빈 등록**
- 빈 이름이 같은 경우 `ConflictingBeanDefinitionException`예외 발생<br/>

**2. 자동 빈 등록 vs 수동 빈 등록**
- 수동 빈 등록이 우선 진행: 수동빈이 자동 빈을 오버라이딩
- 하지만 최근 스프링 부트는 에러가 발생하도록 바뀜


##### [출처]
- [인프런 스프링 핵심 원리 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)