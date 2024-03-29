---
layout: post
title:  "[Kubernetes] 쿠버네티스 기본 내용"
author: yj
category: [ DevOps☁️]
tags: [ Docker ]
---

## 컨테이너란?

### 가상 머신 vs 컨테이너

**가상 머신**

- 하드웨어 인프라 위에 Hypervisor(vmware, virtual box)를 설치함
- 이후 소프트웨어 기술을 이용해 가상의 머신을 만듦
- 가상의 머신에 OS를 설치하고 애플리케이션을 동작시키는 형태
→ 가장 적당한 하드웨어 리소스를 할당하므로 하드웨어를 더 효율적으로 쓰게 한다.

**컨테이너**

- Infrastructure위에 Host Operation(Linux 등) 설치
- 그 위에 Docker를 올려 docker daemon start

**차이점**

- 가상머신에서는 여러 애플리케이션을 돌리기 위해 OS, 메모리를 매번 할당해야한다.
- 컨테이너는 소스코드와 베이스 환경만 있다면 동작시킬 수 있음
- 컨테이너가 훨씬 가볍기때문에 빠르게 확장, 배포가 가능하다.

## Kubernetes

- 컨테이너화된 애플리케이션을 자동으로 배포, 스케일링 및 관리해주는 오픈소스 시스템
- 컨테이너 오케스트레이션
    - 어떻게 애플리케이션을 구성했을 때 가장 최적의 환경이 되는지 분석해여 배치
- Contorl plane이 Worker 노드를 관리해주는 구조

**특징**

- 워크로드의 분리: 애플리케이션이 분리되어 실행되지만 컨테이너간 통신이 가능하다.
- 어디에서나 실행 가능: 환경에 상관 없이 어디에서나 실행할 수 있다.
- 선언적 API: 알아서 현재의 상태를 판단해 요청을 수행
    - NoOps: Ops없이 개발이 가능하도록 한다.