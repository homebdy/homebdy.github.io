---
layout: post
title:  "[Etc] github Action"
author: yj
category: [ Etc💬 ]
tags: [ etc, gitgub, deploy ]
---

### <a href="#">github Action 이란?</a>
- Github에서 제공하는 CI/CD를 위한 서비스
- 코드 저장소에서 이벤트(commit, PR 등...)가 발생하면 특정 작업이 일어나도록 설정 가능
- 주기적으로 어떠한 작업 반복 실행 설정 가능
- 새로운 코드가 main branch에 push될 경우 해당 부분을 다시 build하고 서버에 배포 가능

### <a href="#">github Action의 구성 요소</a>

##### 1. Workflow
- 자동화 해놓은 작업 과정
- 코드 저장소 내에서 `.github/workflows` 폴더 내 위치한 yaml 파일로 설정
- 하나의 코드 저장소 내에 여러 워크플로우 생성 가능

##### 2. Events
- workflow의 실행을 유발하는 특정 행위
- 커밋 푸쉬 or issue 생성, PR 요청 등

##### 3. Jobs
- workflow의 기본 단위
- 여러 작업을 병렬적으로 실행하며 순차적 실행도 설정 가능

##### 4. Actions
- workflow의 가장 작은 요소
- 직접 만들어 사용 가능 or 이미 작성된 것을 가져다 사용 가능

### <a href="#">Github Action 흐름</a>

1. 코드 작성
2. workflow 정의
3. Test