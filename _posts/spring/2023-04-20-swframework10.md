---
layout: post
title:  "REST 아키텍처와 예외처리"
author: yj
category: [ Spring🌱 ]
tags: [ JAVA, SPRING, MVC ]
---

## <a href="#">REST 아키텍처</a>

클라이언트와 서버 사이 데이터 연동 애플리케이션을 위한 아키텍처 스타일

- 웹 상의 정보를 리소스로 파악하고 그 식별자로서 URI를 할당해 고유하게 주소를 지정하는 방식
- HTTP 프로토콜을 사용해 리소스에 접근하고 HTTP 메서드에 대한 처리 결과를 서버에서 JSON또는 XML 형식으로 전송한다.
- 리소스 포맷으로는 일반적으로 JSON, XML 방식을 사용한다.

**HTTP 메서드**

1. GET: URI에 지정된 리소스를 가져온다.
2. POST: 리소스를 생성하고 생성된 리소스에 접근할 수 있는 URI를 받아온다.
3. PUT: URI에 지정된 리소스를 생성하거나 갱신한다.
4. DELETE: URI에 지정된 리소스를 삭제한다.

**Spring에서 REST 컨트롤러를 사용하기 위한 라이브러리 설정**

- 리소스 형식을 JSON으로 사용해야한다.
- Jackson-databind 사용: JSON 형식과 자바 빈즈 상호 변환
    ```
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.12.6</version>
    </dependency>
    ```

**REST 컨트롤러에 사용되는 애노테이션**

1. `@RestController`: Rest API를 제공하는 컨트롤러
    - @Controller와 @ResponseBody를 합친 기능
2. `@RequestBody`: 컨트롤러 메서드 매개 변수에 사용하는 애노테이션
    - spring은 요청된 HTTP request body를 해당 매개 변수에 바인딩 한다.
3. `@ResponseBody`: 컨트롤러 메소드에 사용하는 어노테이션
    - 스프링이 반환 값을 나가는 HTTP response body에 바인딩
    - 요청된 메시지의 HTTP 헤더에 있는 Content-Type을 기반으로 HTTP Message converter를 사용하여 반환 값을 HTTP response body로 변환
4. `ResponseEntity`: 전체 HTTP 응답을 나타내며 statusCode, headers, body 3가지 속성 값 지정하는 기능

**예시 코드**

```java
@RequestMapping(value = "/{id}", method = RequestMethod.GET)
public ResponseEntity<StudentVO> readMember(@PathVariable String id) throws Exception {
    StudentVO student = memberService.readMember(id);
    logger.info(" /member/rest/{id} REST-API GET method called. then method executed.");
    HttpHeaders headers = new HttpHeaders();
    headers.setContentType(new MediaType("application", "json", Charset.forName("UTF-8")));
    headers.set("My-Header", "MyHeaderValue");
    return new ResponseEntity<StudentVO>(student, headers, HttpStatus.OK); 
}
```

## <a href="#">예외 처리</a>

**예외 처리 방식**

1. 컨트롤러 별 예외 처리
- 컨트롤러 메서드에서 예외가 발생했을 때의 처리 정의
- 별도의 예외 처리 메서드를 정의하고 그 메서드에 `@ExceptionHandler` 애노테이션 설정

2. 하나의 웹 애플리케이션 안에서 공통된 예외처리
- 복수의 컨트롤러에서 사용할 수 있는 공통된 예외 처리 클래스를 정의
- 공통된 예외 처리 클래스를 정의하고 해당 클래스에 `@ControllerAdvice` 애노테이션 설정