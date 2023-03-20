---
layout: post
title:  "Meaven과 Gradle"
author: yj
category: [ Spring🌱 ]
tags: [ JAVA, SPRING ]
---

## <a href="#">Maven</a>

프로젝트를 관리하는 도구로 빌드 자동화 기능과 프로젝트 관리 기능을 제공

> **1. 프로젝트 관리**
>   - pom.xml을 이용해 프로젝트 관련 jar파일을 관리
>   - 프로젝트의 산출물을 인관된 구조로 관리
>
> **2. 빌드 자동화**
>   - 빌드 작업들을 간단하고 쉽고 일관성 있게 수행할 수 있는 환경 제공
>   - 빌드: 소스 코드 파일을 실행 코드로 변환하여 배포하는 과정

#### 프로젝트 관리 기능

일반 개발자 프로젝트 관리 설정들을 메이븐이 미리 정의한 설정들로 대체한다

1. 정형화된 프로젝트 디렉토리 구조 관리
    - Convention over Configuration(CoC) 패러다임을 따름: 설정보다는 규범

2. 의존성 관리 기능: 편리한 라이브러리 관리 기능
    - 프로젝트 빌드에 필요한 라이브러리, 플러그인을 개발자 PC에 자동 다운로드

3. 빌드 프로세스 관리
    - 플러그인 설정을 통해 빌드 자동화

**프로젝트 디렉토리 기본 설정**
- src/main/resources: classpath로 사용되는 디렉토리로 주요 설정 정보를 저장
- src/main/java: source code 저장
- src/main/tests: 테스트 코드 저장
- target: jar 파일 저장

**의존 관계 설정**

`pom.xml` 파일에서 관리
- 3개의 필수 필드를 가지고 관리한다.
    1. groupId: 프로젝트 조직 고유 도메인
    2. artifactId: 프로젝트 명
    3. version: 프로젝트 버전

**프로젝트 빌드 설정**

프로젝트 기본 정보, 저장소, 프로퍼티, 디렉토리 구조 설정
- plugins
- goals

**pom.xml**
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>
    <groupId>그룹 아이디</groupId>
    <artifactId>프로젝트 이름</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>maven</name>
    <url>http://maven.apache.org</url>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>3.8.1</version>
            <scope>test</scope>
        </dependency>
    </dependdencies>
</project>
```

모든 pom은 Super pom으로 부터 상속받는다
- super pom: 기본 설정 정보 포함
- super pom을 수정하기 위해 pom에서 오버라이드 해야함

#### Meaven Repository

1. 지역 저장소

    메이븐을 빌드할 때 다운로드하는 라이브러리와 플러그인을 관리하는 저장소

    - 구조: `USER_HOME/.m2/repository`에 존재
    - groupId와 artifactId 아래에 각 버전에 따른 라이브러리 관리

2. 중앙 저장소
    - 오픈 소스 라이브러리와 메이븐 플러그인, 아키타입을 관리하는 저장소

3. 원격 저장소
    - 중앙 저장소에 존재하지 않는 라이브러리를 관리하기 위해 별도의 메이븐 저장소를 설치해 관리하는 저장소

**의존성 검색 절차**

1. 지역 저장소 검색
    - 찾는 라이브러리가 없는 경우 2단계로 넘어간다.

2. 중앙 저장소 검색
    - 찾은 경우: 라이브러리를 지역 저장소에 저장
    - 찾지 못한 경우: 원격 저장소가 존재하면 3단계로 넘어가고, 없을 경우는 에러 발생 후 종료

3. 원격 저장소 검색
    - 찾은 경우: 지역 저장소에 저장
    - 찾지 못한 경우: 에러 발생 후 종료

**의존 라이브러리 적용 범위**

의존 라이브러리를 적용할 수 있는 시점을 제한할 수 있다.

1. Compile: 스코프를 설정하지 않은 경우 기본 스코프
2. provided
    - 컴파일: 직접 의존성 참조
    - 런타임: 해당 컨테이너의 서블릿 API에서 의존성을 제공 받는다.
3. runtime
    - 컴파일: 사용하지 않음
    - 애플리케이션이 실행될 때만 사용
4. test: 테스트 시점에만 사용
5. system: provided와 유사
     - 직접 jar파일을 제공해야 한다.
6. import: 다른 pom 설정 파일에 정의되어 있는 의존 관계 설정을 현재 프로젝트로 가져온다.

#### 빌드 자동화 기능

Maven은 플러그인 설정을 통해 기능 위임

- 빌드 단계 (컴파일, 테스트, 패키징, 배포)를 `빌드 라이프 사이클` 이라 한다.
- `goal`: 각 빌드 단계에서 수행되는 작업
- goal은 그 단계에 연결된 `plugin`에 의해 실행

**빌드 라이프 사이클**

build, clean, site 3개의 라이프 사이클이 존재

1. build: 여러 단계의 phase로 나뉘어져 있음
    - 각 페이즈는 의존 관계를 가진다.
    - compile, test, package deploy 순으로 진행

2. clean
    - 이전 빌드에서 생성된 모든 파일들을 삭제
    - target 디렉토리에 존재하는 파일을 삭제한다.

3. site
    - site, site-deploy를 이용해 생성된 문서들을 대상 사이트에 배포한다.

**빌드 사이클의 일부 단계**

1. 리스스 준비: 리소스가 준비되어 복사된다.
2. 컴파일: 소스 코드를 컴파일한다.
3. 테스트: Junit과 같은 테스트 프레임워크로 단위 테스트 실행
4. 패키지: pom.xml의 \<packaging/> 엘리먼트 값에 따라 압축 진행
5. 인스톨: 패키지를 로컬 저장소에 배포
6. 배포: 원격 meaven 저장소에 압축한 파일 배포

**phase와 Goal**

빌드 라이프사이클은 하나 이상의 골을 수행하는 phase들로 구성
- 각 phase 별로 플러그인이 작업을 수행
- 이 작업을 `goal`이라 함
- 과정
    1. Compile phase
        pom.xml 파일의 \<plugin></plugin> 플러그인 작업 수행
    2. test, package, ... 단계 수행

**Plugin**

대부분의 기능들 제공

1. clean: 빌드 이후 target 디렉토리 삭제
2. compiler: 자바 소스 파일 컴파일
3. surefire: JUnit 단위 테스트 실행 후 리포트 생성
4. jar: 현재 프로젝트로 부터 jar 파일 생성
5. war: 현재 프로젝트로 부터 war 파일 생성
6. javadoc: 프로젝트의 Javadoc문서 생성
7. antrun: 빌드 페이지 단계에서 특정 ant 작업 실행

## <a href="#">Gradle</a>

- JVM에서 실행
- Maven 처럼 Convention을 통해 프로젝트 생성과 빌드 수행
- Maven처럼 저장소 관리 기능 제공
- 다양한 플러그인 제공
- 빌드 설정 스크립트를 Groovy 기반의 DSL 사용
- Gradle 설치 없이 Gradle Wrapper 이용하여 빌드 지원
- 하나의 repository 내에 여러개의 하위 프로젝트를 구성해 멀티 프로젝트 구성 가능

#### Gradle의 구조

1. gradle-wrapper.jar: gradle task 실행을 위한 Wrapper 파일
    - wrapper은 선언된 버전의 gradle을 호출해 필요한 경우 미리 다운로드
    - 프로젝트를 새로운 환경에 종속되지 않고 빌드할 수 있도록 한다.

2. gradle-wrapper.properties: gradle wrapper 설정 정보 파일

3. build.gradle: 프로젝트의 라이브러리 의존성, 플러그인, 라이브러리 저장소 등을 설정할 수 있는 빌드 스크립트 파일
4. gradlew: 리눅스 또는 맥 OS 용 실행 스크립트 파일
5. gradlew.bat: 윈도우용 실행 스크립트 파일
6. setting.gradle: 프로젝트의 설정 정보 파일

**build.gradle 설정**

- Plugin: 작업 수행 환경 명시
- Version, Group: 패키지 상의 적용 범위와 어플리케이션 버전, Java 버전 명시
- Profile: 개발 환경에 따른 profile 환경 파일
- BuildScript: 앞서 선언한 플러그인의 의존성 관리
- dependencies(library): 기본적인 프러그인 이외 개발에 필요한 추가 라이브러리 의존성 관리

```
plugins {
    id 'java-library'
} // 플러그인 설정

repositories {
    mavenCentral()
} // repository 설정: 의존관계를 내려받기 위한 저장소 지정

dependencies {

} // dependency 설정
```

**plugin**

task들의 집합

- java-library 플러그인 등록 설정
    - 자바 플러그인의 기능을 확장하여 자바 라이브러리에 대하여 API 형식으로 제공 가능

**dependency 의존관계 설정**

1. implementatin: 코드 컴파일 과정과 필요 시 런타임에 필요한 라이브러리 의존성 추가
2. compileOnly: 컴파일 과정에마 필요한 라이브러리 의존성 추가
3. compileclasspath: 코드 컴파일 시점의 compile classpath를 의미
4. annotationProcessor: 컴파일 시점에 어노테이션이 붙은 코드에 대해 코드 생성
5. runtimeOnly: 코드 실행시 런타임에만 적용
6. runtimeClasspath: 코드 런타임에서 사용하는 runtimeClasspath 의미