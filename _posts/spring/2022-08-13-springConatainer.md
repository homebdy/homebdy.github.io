---
layout: post
title:  "스프링 컨테이너와 스프링 빈"
author: yj
category: [ Spring🌱 ]
tags: [ JAVA, SPRING ]
---
### <a href="#">스프링 컨테이너 생성</a>
- 스프링 컨테이너가 생성되는 과정
```java
    ApplicationContext applicationContext 
        = new AnnotationConfigApplicationContext(AppConfig.class);
```
- `ApplicationContext`를 스프링 컨테이너라고 함
- `Applicationcontext`는 인터페이스
- 스프링 컨테이너는 XML 기반으로 만들 수 있고, 에노테이션 기반의 자바 설정 클래스로도 만들 수 있음

- 스프링 컨테이너 생성 과정<br/>


    1. 스프링 컨테이너 생성
        - `new AnnotationConfigApplicationContext(AppConfig.class)`
        - 내부에 스프링 빈 저장소 생김
        - App의 구성 정보 활용
    <br/><br/>
    2. 스프링 빈 등록: 스프링 컨테이너는 파라미터로 넘어온 설정 클래스 정보를 사용해 스프링 빈 등록
        - 스프링 빈의 이름은 대부분 그냥 메서드 이름을 사용함
        - 빈 이름을 직접 부여할 수 있지만 항상 다른 이름을 부여해야 한다.
    <br/><br/>
    3. 스프링 빈 의존관계 설정 준비 & 설정
        - 설정 정보를 참고해 의존관계를 주입
        - 스프링은 빈을 생성하고 의존 관계를 주입하는 단계가 나누어져 있다. 


### <a href="#">스프링 빈 조회</a>

1. ac.getBean(빈이름, 타입)
2. ac.getBean(타입)
3. 조회 대상 스프링 빈이 없으면 `NoSuchBeanDefinitionException` 발생

```java
        @Test
        @DisplayName("빈 이름으로 조회")
        void findBeanByName() {
            MemberService memberService = ac.getBean("memberService", MemberService.class);

            assertThat(memberService).isInstanceOf(MemberServiceImpl.class);
        }

        @Test
        @DisplayName("이름 없이 타입으로만 조회")
        void findBeanByType() {
            MemberService memberService = ac.getBean(MemberService.class);

            assertThat(memberService).isInstanceOf(MemberServiceImpl.class);
        }
```
<br/>

**스프링 빈 조회: 동일타입이 존재하는 경우**<br/>

&nbsp;&nbsp;&nbsp;&nbsp;→ 오류 발생. 빈 이름을 지정해야함<br/>
&nbsp;&nbsp;&nbsp;&nbsp;→ `ac.getBeansOfType()` 사용 시 해당 타입의 모든 빈 조회 가능
```java 
    @Test
    @DisplayName("타입 조회 시 같은 타입이 둘 이상 존재하면 중복 오류 발생")
    void findBeanByTypeDuplicate() {
        Assertions.assertThrows(NoUniqueBeanDefinitionException.class,
                () -> ac.getBean(MemberRepository.class));
    }

    @Test
    @DisplayName("특정 타입 모두 조회")
    void findAllBeanByType() {
        Map<String, MemberRepository> beansOfType = ac.getBeansOfType(MemberRepository.class);
        for (String key : beansOfType.keySet()) {
            System.out.println("key = " + key + " value = " + beansOfType.get(key));;
        }
        System.out.println("beansOfType = " + beansOfType);
        org.assertj.core.api.Assertions.assertThat(beansOfType.size()).isEqualTo(2);
    }
```
<br/>

**스프링 빈 조회: 상속 관계** <br/>
- 부모 타입으로 조회할 경우 자식타입도 함께 조회가 된다.
- 따라서 모든 객체의 최고 부모임 `Object` 타입으로 조회하면 모든 스프링 빈 조회 가능
```java 
    @Test
    @DisplayName("Object 타입으로 모두 조회")
    void findAllBeanObject() {
        Map<String, Object> beanOfType = ac.getBeansOfType(Object.class);
        for (String key : beanOfType.keySet()) {
            System.out.println("key = " + key + " value = " + beanOfType.get(key));
        }
    }
```


### <a href="#">BeanFactory와 ApplicationContext</a>

**BeanFactory**
- 스프링 컨테이너의 최상위 인터페이스
- 스프링 빈 관리 및 조회 역힐
- `getBean()` 제공

**ApplicationContext**
- BeanFactory 기능을 모두 상속받아 제공
- 대부분 BeanFactory를 직접 사용하지 않고 부가 기능이 포함 된 ApplicationContext를 사용
- BeanFactory와 차이 - 애플리케이션 개발 시 필요한 부가기능을 제공<br/>
    > 예시:<br/>
    > 메시지소스를 활용한 국제화 기능<br/>
    > 환경변수: 로컬, 개발, 운영 등을 구분해서 처리<br/>
    > 애플리케이션 이벤트: 이벤트 발행하고 구독하는 모델 편리하게 지원<br/>
    > 편리한 리소스 조회: 파일, 클래스 패스, 외부 등에서 편리하게 조회<br/>

### <a href="#">XML</a>
- 스프링 컨테이너는 다양한 형식의 설정 정보를 받아드릴 수 있도록 설계

**XML 설정 사용**
- `GenerictXmlApplicationContext`를 사용하며 xml 설정 파일을 넘긴다.<br/><br/>
```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

        <bean id="memberService" class="hello.core.member.MemberServiceImpl">
            <constructor-arg name="memberRepository" ref="memberRepository" />
        </bean>

        <bean id="memberRepository" class="hello.core.member.MemoryMemberRepository"/>

        <bean id="orderService" class="hello.core.order.OrderServiceImpl">
            <constructor-arg name="memberRepository" ref="memberRepository"/>
            <constructor-arg name="discountPolicy" ref="discountPolicy"/>
        </bean>

        <bean id="discountPolicy" class="hello.core.discount.RateDiscountPolicy" />
    </beans>
```

##### [출처]
- [인프런 스프링 핵심 원리 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8)