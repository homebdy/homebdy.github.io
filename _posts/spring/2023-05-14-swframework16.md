---
layout: post
title:  "AOP - Annotation, Java 방식 구현 방법"
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

### 2. Annotation을 이용한 방식

#### Annotation 설정

1. DI Anootation 설정
    ```java
    @Component
    public class MemberServiceImpl implements MemberService {

        @Autowired
        private MemberDAO memberDAO;

        public StudentVO readMember(String userid) throws Exception {
            return memberDAO.read(userid);
        }

        public void addMember(StudentVO student) throws Exception {
            memberDAO.add(student);
        }
    }
    ```

2. applicationContext.xml 설정: DI, AOP 설정

```xml
<context:component-scan base-package="org.tukorea.di.persistence"></context:component-scan>
<context:component-scan base-package="org.tukorea.di.service"></context:component-scan>
<context:component-scan base-package="org.tukorea.aop"></context:component-scan>
```

#### Aspect 정의 클래스

```JAVA
@Aspect
@Component
Public class MemberAspect {

    @Before("execution(* read(String))")
    public void beforeMethod(JoinPoint jp) {
        System.out.println(” [BeforeMethod] : 메소드 호출 전 ”);
        Signature sig = jp.getSignature();
        System.out.println(” 메소드 이름: ” + sig.getName());
        Object[] obj = jp.getArgs();
        System.out.println(” 인수 값: ” + obj[0]);
    }

    @After("execution(* read(String))")
    public void afterMethod() {
        System.out.println(” [AfterMethod] : 메소드 호출 후 ”);
    }

    @AfterReturning(value = "execution(* read(String))", returning = "member")
    public void afterReturningMethod(JoinPoint jp, StudentVO member) {
        System.out.println(” [afterReturningMethod] : 메소드 호출 후 ”);
        Signature sig = jp.getSignature();
        System.out.println(” 메소드 이름: ” + sig.getName());
        Object[] obj = jp.getArgs();
        System.out.println(” 인수 값: ” + obj[0]);
    }

    @Around("execution(* read(String))")
    public StudentVO aroundMethod(ProceedingJoinPoint pjp) throws Throwable {
        System.out.println("[AroundMethod before] : 메소드 호출 전");
        StudentVO member = (StudentVO) pjp.proceed();
        System.out.println("[AroundMethod after] : 메소드 호출 후");
        return member;
    }

    @AfterThrowing(value = "execution(* read(String))", throwing = "ex")
    public void afterThrowingMethod(Throwable ex) {

        System.out.println(" [AfterThrowingMethod] : 예외 발생 후");
        System.out.println(" exception value = " + ex.toString());
    }
}

```

- Aspect 선언: `@Aspect`
- DI 컴포넌트 선언: `@Component`
- Advice 선언: @Before, @After, @AfterReturning, @Arround, @AfterThrowing
- Pointcut 선언: @Pointcut( “execution( )”)

#### Annotation으로 advice 설정

1.  `@Before("execution(* read(String))")`, `@After("execution(* read(String))")`
    - 반환형: void, 매개변수 없음
    - pointcut: : "execution(* read(String))"

2. `@AfterReturning(value = "execution(* read(String))", returning = "member")`
    - 메서드 반환형 정의: retuning = "member"
    - pointcut : "execution(* read(String),", returning = "member“) 

3. `@Around("execution(* read(String))")`
    - 메서드의 반환형: AOP 대상의 메서드 반환형과 호환
    - pointcut : "execution(* read(String))"

4. `@AfterThrowing(value = "execution(* read(String))", throwing = "ex")`
    - 메서드의 매개변수에 캐치할 예외를 선언
    -  pointcut : "execution(* read(String))"

### 3. JAVA를 이용한 방식

#### JAVA 코드 설정

- JavaConfig로 AOP를 정의하기 위해서는 @EnableAspectAutoProxy 선언 필요
    : 빈 정의 파일에서 aop:aspectj-autoproxy 태그와 같은 역할


    ```java
    @Configuration
    @EnableAspectJAutoProxy
    public class JavaConfig {

        @Bean
        public MemberDAO memberDAO() {
            return new MemberDAOImpl();
        }

        @Bean
        public MemberService memberService() {
            return new MemberServiceImpl(memberDAO());
        }
        
        @Bean
        public MemberAspect memberAspect() {
            return new MemberAspect();
        }
    }
    ```