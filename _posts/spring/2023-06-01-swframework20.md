---
layout: post
title:  "Spring Security"
author: yj
category: [ Spring🌱 ]
tags: [ JAVA, SPRING ]
---


## <a href="#">Spring Security</a>

### 인터셉터

Spring MVC의 인터셉터는 Spring MVC는 Controller의 호출 이전과 이후에 추가적인 기능을 할 수 있는 구조를 제공한다.
- 기능상으로는 AOP의 기능(범용적)과 유사하다.
- 구조상으로는 서블릿/JSP에서 사용하는 Filter와 유사하다. (특정 URL의 접근 제어가 가능)

### 필터와 인터셉터

**영역제어측면에서의 차이점**

- 인터셉터는 스프링 빈으로 등록된 컨트롤러나 서비스 객체를 주입 받는다.

**인터셉터 처리 순서**

- 필터는 Dispatcher Servlet이 호출
- Handler Interceptor는 Dispatcher Servlet 가 호출되고 난 후에 호출

### Spring AOP 기능과 HandlerInterceptor의 차이

- Interceptor 파라미터가 HttpServletRequest, HttpServletResponse라는 점
    - AOP는 (JointPoint, PreceedingJointPoint) 등의 파라미터를 활용하는 방식
    - AOP는 특정 객체 동작의 사전, 사후 처리를 활용 

- Interceptor는 DispatcherServlet이 Controller를 호출하기 전과 후에 요청과 응답을
참조하거나 적용할 수 있는 일종의 서블릿 필터
    - Spring에서 Interceptor는 HandlerInterceptor(Interface)를 구현하거나 또는 HandlerInterceptorAdapter(Abstract Class)를 상속 받아 구현함
    - HandlerInterceptorAdapter는 HandlerInterceptor 인터페이스를 구현한 추상 클래스
    - HandlerInterceptorAdapter is deprecated : 스프링 5.3, 스프링 부트 2.4 부터

**HandlerInterceptor 인터페이스를 구현**

- 컨트롤러 실행 전 : preHandle(request, response, handler)
- 컨트롤러 실행 후 : postHandle(request ,response, handler, modelAndView)
- 뷰를 실행한 이후 : afterCompletion(request, response, handler, exception)

**컨트롤러 실행 전 메서드**

`preHandle(request, response, handler)`

- 컨트롤러(핸들러) 객체를 실행하기 전에 필요한 기능을 구현할 때 사용.
- 접근 권한이 없는 경우 컨트롤러를 실행하지 않거나, 컨트롤러에 필요한 정보를 생성하는 작업이 가능함
- 리턴타입이 boolean인데, false를 리턴하면 컨트롤러를 실행하지 않는다.

**컨트롤러 실행 후 메서드**

`postHandle(request, response,handler, modalAndView)`

- 컨트롤러 객체가 정상 실행 된 후(화면 처리 전)에 필요한 기능을 구현할 때 사용
- 컨트롤러가 익셉션이 발생하면 실행되지 않는다.

**뷰를 실행한 이후 메서드**

`afterCompletion(request, response, handler, exception)`

- 클라이언트에 뷰를 전송한 뒤(화면 처리 후) 실행된다.
- 뷰를 처리한 후 처리 시간 기록등 후처리에 적합한 메서드
- 컨트롤러가 익셉션이 발생하면 exception 파라미터를 통해 예외 값이 전달 없으면 null