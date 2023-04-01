---
layout: post
title:  "빈 객체의 스코프와 라이프사이클"
author: yj
category: [ Spring🌱 ]
tags: [ JAVA, SPRING ]
---

## <a href="#">빈 객체 스코프(Scope)</a>

빈이 생성될 수 있는 범위를 설정하기 위해서는 scope 속성을 사용

1. singleton: 기본 설정
- 컨테이너 당 한개의 빈 객체만 생성

2. prototype
- 빈을 요청할 때마다 빈 객체를 생성

3. request
- 각 요청용으로 한 개의 빈 객체를 생성

4. session
- 각 세션용으로 한 개의 빈 객체를 생성

5. application
- 서블릿 컨텍스트가 생성될 때 빈 객체 생성

### 빈 객체의 라이프 사이클

빈 객체의 라이프 사이클은 초기화 ⇀ 이용 ⇀ 종료 3단계 진행

- 빈 생성 후 초기화 작업과 빈 종료 전 전처리 과정을 수행할 수 있는 방법 제공

**빈 생성 후 초기화**

1. XML 기반: \<bean> 요소의 init-method 속성에 메서드 지정
2. Annotation 기반:  @PostConstruct annotation 붙은 메서드
3. Java: @Bean의 initMethod 속성에 지정된 메서드
4. 인터페이스 구현: InitialzeBean 인터페이스의 afterPropertiesSet 메서드

**빈 종료 전 전처리**

1. XML 기반: \<bean> 요소의 destroy-method 속성에 메서드 지정
2. Annotation 기반: @PreDestroy 애너테이션 붙은 메서드
3. Java: @Bean의 destroyMethod 속성에 지정된 메서드
4. 인터페이스 구현: DisposableBean 인터페이스의 destroy 메서드

**실행 형태**

```java
import org.springframework.beans.factory.DisposableBean;
import org.springframework.beans.factory.InitializingBean;

public class MyBeanImpl implements LifeBean, InitializingBean, DisposableBean {
	//InitializingBean 인터페이스 메소드
	@Override
	public void afterPropertiesSet() throws Exception {
		.................................
	}
	//DisposableBean 인터페이스 메소드
	@Override
	public void destroy() throws Exception {
		...................................
	}
}
```

**TEST**

```java
package org.tukorea.di.service;
import org.tukorea.di.domain.StudentVO;
import org.tukorea.di.persistence.MemberDAO;
import org.springframework.beans.factory.DisposableBean;
import org.springframework.beans.factory.InitializingBean;

public class MemberServiceImpl implements MemberService, InitializingBean,DisposableBean {
	private MemberDAO memberDAO;
	.....................................
	@Override
	public void afterPropertiesSet() throws Exception {
		System.out.println("Init MemberServiceImple");
	}
	@Override
	public void destroy() throws Exception {
		System.out.println("Destroy MemberServiceImple");
	}
}
```