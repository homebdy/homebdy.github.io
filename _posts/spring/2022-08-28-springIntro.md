---
layout: post
title:  "스프링 웹 개발 기초"
author: yj
category:  Spring🌱
tags: [ JAVA, SPRING ]
---

### <a href="#">정적 컨텐츠</a>
- html 파일 그대로 화면에 띄우는 컨텐츠
**과정**
1. 웹 브라우저의 요청
2. 서버는 스프링 컨테이너에서 관련 컨트롤러를 찾는다.
3. 컨트롤러가 존재하지 않는 경우 정적 컨텐츠 파일을 찾아 그대로 반환

### <a href="#">MVC</a>
- Model, View, Controller
**과정**
1. 웹 브라우저의 요청
2. 서버는 스프링 컨테이너에서 관련 컨트롤러를 찾음
3. 컨트롤러에서 요청 처리 후 리턴한 값은 viewResolver가 html파일에 담아 반환

### <a href="#">API</a>
1. `@ResponseBody` 문자 반환 - viewResolver 사용 X
- HTTP의 Body에 문자 내용을 직접 반환

2. `@ResponseBody` 객체 반환
- 객체가 Json형식으로 변환
- JsonConverter가 처리하는 내용