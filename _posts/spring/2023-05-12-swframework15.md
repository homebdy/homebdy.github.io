---
layout: post
title:  "AOP - xml 방식 구현 방법"
author: yj
category: [ Spring🌱 ]
tags: [ JAVA, SPRING ]
---


## <a href="#">AOP 구현 3가지 방법</a>

1. XML을 이용한 AOP 구현
- XML 스키마 기반의 POJO클래스를 이용한 구현 방식

2. Annotation을 이용한 AOP 구현
- 애노테이션 기반의 구현 방식

3. 자바 코드를 이용한 AOP 구현
- JAVA 기반의 구현 방식

### 1. XML 방법

- MemberAspect.java: aspect 정의 클래스

    ```java
    public class MemberAspect {
        public void beforeMethod(JoinPoint jp) {
                System.out.println("[BeforeMethod] : 메소드 호출 전");
                Signature sig = jp.getSignature();
                System.out.println(" 메소드 이름:" + sig.getName());
                Object[] obj = jp.getArgs();
                System.out.println(" 인수 값:" + obj[0]);
        }
        public void afterMethod() {
                System.out.println("[AfterMethod] : 메소드 호출 후");
        }
        public void afterReturningMethod(JoinPoint jp, StudentVO member) {
        
                System.out.println("[afterReturningMethod] : 메소드 호출 후");
                Signature sig = jp.getSignature();
                System.out.println(" 메소드 이름:" + sig.getName());
                Object[] obj = jp.getArgs();
                System.out.println(" 인수 값:" + obj[0]);
        }
        public StudentVO aroundMethod(ProceedingJoinPoint pjp) throws Throwable {
                System.out.println("[AroundMethod before] : 메소드 호출 전");
                StudentVO member = (StudentVO) pjp.proceed();
                System.out.println("[AroundMethod after] : 메소드 호출 후");
                return member;
        }
        public void afterThrowingMethod(Throwable ex) {
                // 메소드 호출이 예외를 내보냈을 때 호출되는 Advice
                System.out.println("[AfterThrowingMethod] : 예외 발생 후");
                System.out.println("exception value = " + ex.toString());
        }
    }
    ```

-  각 메서드가 advice
    1.  JoinPoint
        - 메서드 명과 인수 값 접근가능
    2. afterReturning 어드바이스
        - 메서드 수행 후 반환한 값 접근 가능
    3.  around 어드바이스
        - proceed 함수를 통해 메서드 수행을 반드시 처리
        - 반환 값 반드시 리턴
        - before와 after를 처리 가능
    4. Throwing 어드바이스
        - AOP 대상이 되는 메서드에서 예외가 발생할 경우만 동작

### 빈 정의 파일(applicationContext.xml)의 AOP 태그

1. aop:config aop 정의를 위한 최상위 엘리먼트
2. aop:aspect aspect를 정의한다. 
    - 속성: id(구별자), ref(bean 태그 지정된 id)
3. aop:pointcut pointcut을 정의한다. 
    - 속성: id(구별자), expression(포인터컷 기술)
4. aop:before before 어드바이스를 정의한다. 
    - 속성: pointcut-ref, method(적용 메서드이름)
5. aop:after after 어드바이스를 정의한다. 
    - 속성: pointcut-ref, method(적용 메서드이름)
6. aop:after-returning after-returning 어드바이스를 정의한다. 
    - 속성: pointcut-ref, method(적용 메서드이름)
7. aop:around around 어드바이스를 정의한다. 
    - 속성: pointcut-ref, method(적용 메서드이름)
8. aop:after-throwing after-throwing 어드바이스를 정의한다. 
    - 속성: pointcut-ref, method(적용 메서드이름) 

### applicationContext.xml 설정

```xml
<bean id="memberAspect" class="org.tukorea.aop.MemberAspect">
</bean>
<aop:config>
    <aop:aspect id="testAspect" ref="memberAspect">
        <aop:pointcut id="readMethod" expression="execution(* read(String))"/>
        <aop:before pointcut-ref="readMethod" method="beforeMethod" />
        <aop:after pointcut-ref="readMethod" method="afterMethod"/>
        <aop:after-returning pointcut-ref="readMethod" method="afterReturningMethod" returning="member"/>
        <aop:around pointcut-ref="readMethod" method="aroundMethod"/>
        <aop:after-throwing pointcut-ref="readMethod" method="afterThrowingMethod"
        throwing="ex"/>
    </aop:aspect>
</aop:config>
```

### Advice 코드 실행 흐름(before Advice)

1. 빈 객체를 사용하는 코드에서 스프링이 생성한 AOP 프록시의 메서드를 호출
2. AOP 프록시는 \<aop:before>에서 지정한 메서드를 호출
3. AOP 프록시는 aspect 기능 실행 후, 실제 빈 객체의 메서드를 호출

### pointcut 기술 방법: execution() 표현식

- 대표적인 포인트컷의 지시자
- ex): \<aop:pointcut id= readMethod＂expression=＂execution(* read(String))＂/>
- 메서드의 수식자, throws 는 생략 가능
- 메서드의 반환값, 패키지와 클래스 명은 와일드 카드 `*` 를 이용 가능
- 패키지, 클래스 명은 생략 가능
- 해당 패키지 및 하위 패키지를 포함하여 일치시키려면 `..`를 이용

**exctuion() 표현식 예시**

1. execution(* org.tukorea.di.persistence.MemberDAOImpl.write(..))
    - 리턴 타입과 파라미터 상관 없이, 클래스는 org.tukorea.di.persistence.MemberDAOImpl, 메소드명이 write인 메소드를 선정하는 포인트컷 표현식
2. execution(* write(int, int))
    - 리턴 타입 상관없이, 모든 패키지, 클래스 내, 메소드 이름이 write이며, 두개의 int 타입의 파라미터를 가진 모든 메소드를 선정하는 포인트컷 표현식
3. execution(* *(..))
    - 리턴 타입과 파라미터의 종류, 개수에 상관없이 모든 패키지, 클래스 내의 모든 메소드를 선정하는 포인트컷 표현식

### Pointcut 기술 방법 : within() 표현식

- 특정 타입에 속하는 메소드를 포인트컷으로 설정할 때 사용
- execution()의 여러 조건 중에서 타입 패턴 만을 적용하기 때문에 execution 표현식 문법보다 간단

**표현식 예시**

1. within(org.tukorea.aop..*)
    - org.tukorea.aop 및 모든 서브패키지가 포함하고 있는 모든 메소드
2. within(org.tukorea.aop.*)
    - org.tukorea.aop 패키지 밑의 인터페이스와 클래스에 있는 모든 메소드
3. within(org.tukorea.aop.xml)
    - org.tukorea.aop 패키지의 xml 클래스의 모든 메소드