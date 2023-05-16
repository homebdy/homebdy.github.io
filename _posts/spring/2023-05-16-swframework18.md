---
layout: post
title:  "선언적 트랜잭션 처리"
author: yj
category: [ Spring🌱 ]
tags: [ JAVA, SPRING ]
---


## <a href="#">선언적 트랜잭션 처리</a>

미리 선언된 규칙에 따라 트랜잭션을 제어
- xml, Annotation 설정 방식
- 소스 코드 상 트랜잭션 처리 메서드를 호출하므로 복잡하고 가독성이 떨어짐
```
@Autowired
private PlatformTransactionManager txManager;
    public void updateMemberList(StudentVO student, StudentVO student) throws Exception {
        DefaultTransactionDefinition def = new DefaultTransactionDefinition();
        def.setPropagationBehavior( TransactionDefinition.Propagation.REQUIRED );
        TreansactionStatus status = txManager,getTransaction(def);

        txManager.commit(commit); // 정상 완료시
        txManager.rollback(rollaback); // 예외 발생시
    }
```

- xml 방식: \<tx:advuce />태그를 통해 어드바이스 설정
- Annotation 방식: @Transactional을 통해 처리가 필요한 클래스와 메서드에 설정

#### 1. XML 설정

- \<tx:advice> 태그를 이용하여 어드바이스 설정
- \<aop:config> 태그를 이용하여 AOP 설정를 통해 트랜잭션 적용
```xml
   	<tx:advice id="txAdvice" transaction-manager="transactionManager">
        <tx:attributes>
                <tx:method name="*" 
                		propagation="REQUIRED" 
                		isolation="READ_COMMITTED"
                		timeout = "10" 
                />
        </tx:attributes>
 	 </tx:advice> 

	 <aop:config>
	        <aop:pointcut id="txPointcut" expression="execution(* updateMemberList(..))" />
	        <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointcut" />
	 </aop:config>
	<tx:annotation-driven transaction-manager="transactionManager"/>
```

#### 2. Annotation 설정

- `@Transactional` 애노테이션을 통해 트랜잭션 처리가 필요한 클래스와 메서드에 설정 가능
- 애노테이션 트랜잭션 제어 활성화를 위해 <tx:annotation-driven> 요소 추가
- 만약 @Transactional 애노테이션을 이용하지 않는다면 요소 정의가 필요 없음

```xml
<bean id="transactionManager"
class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"></property>
</bean>

<tx:annotation-driven />
```

**Transactional 우선 순위**

- `메서드`의 설정이 가장 우선 순위가 높다
- `클래스`의 @Transactional 설정이 메서드보다 우선 순위가 낮다
- `인터페이스`의 @Transactional 설정이 가장 우선 순위가 낮다

∴ 메서드 - 클래스 - 인터페이스 순