---
layout: post
title:  "쿠버네티스 아키텍처"
author: yj
category: [ DevOps☁️]
tags: [ kubernetes ]
---

## 쿠버네티스 아키텍처

**Desired State**

1. 상태 체크(Observe)
- 현재 상태와 원하는 상태가 같은지 확인

2. 차이점 발견(Diff)
- 현재 상태와 원하는 상태가 같지 않은 것을 확인한 경우

3. 조치(Act)
- 현재 상태가 원하는 상태가 되도록 조치를 취한다

4. Loop

## Master 상세 

### etcd

- 모든 상태와 데이터 저장
- 분산 시스템으로 구성하여 안전성을 높임(고가용성)
- 가볍고 빠르면서 정확하게 설계(일관성)
- Key-Value 형태로 데이터 저장
- TTL, watch 같은 부가 기능 제공
- 백업은 필수

### API Server

- 상태를 바꾸거나 조회
- etcd와 유일하게 통신하는 모듈
- REST API 형태로 제공
- 권한을 체크하여 적절한 권한이 없을 경우 요청 차단
- 관리자 요청 뿐 아니라 다양한 내부 모듈과 통신
- 수평으로 확장되도록 디자인

### Scheduler

- 새로 생성된 Pod을 감지하고 실행할 노드 선택
- 노드의 현재 상태와 Pod의 요구사항 체크
    - 노드에 라벨 부여 (a-zone, b-zone)

### Controller

- 논리적으로 다양한 컨트롤러가 존재: 복제, 노드, 엔드포인트 ....
- 끊임 없이 상태를 체크하고 원하는 상태를 유지
- 복잡성을 낮추기 위해 하나의 프로세스로 실행

### 흐름

**조회 흐름**

- Controller가 API Server에 정보 조회 요청
- API Server가 정보 조회 권한 체크
- 해당 컨트롤러가 리소스 접근 권한이 있는지 확인
- 권한이 있을 경우 etcd에서 정보를 조회해 반환

**기본 흐름**

- etcd가 API Server에 원하는 상태 변경 내용을 전달
- API Server는 Controller에게 원하는 상태가 변경이 되었다 알림
- Controller에서 원하는 상태로 리소스 변경
- Controller가 API Server에 변경 사항 전달
    - 해당 컨트롤러가 정보 갱신 권한이 확인
- 권한이 있을 경우 etcd에서 정보를 갱신

## 노드

**Kubelet**

- 각 노드에서 실행
- Pod을 실행/중지하고 상태 체크
- CRI(Container Runtime Interface): docker, containered, CRI-O

**Proxy**

- 네트워크 프록시와 부하 분산 역할
- 성능상의 이유로 별도의 프록시 프로그램 대신 iptables 또는 IPVS 사용 - 설정만 관리

## 쿠버네티스 흐름

1. 관리자가 API Server에 Pod 요청
2. API Server는 etcd에 Pod 요청
3. Controller가 새 Pod 확인, 발견
4. API Server는 etcd에 Pod 할당 요청
5. Scheduler: 새 Pod 할당 확인 요청
6. 새 Pod 할당 확인
7. 특정 노드에 Pod 할당
8. Pod 노드 할당
9. kubelet: 미실행 Pod 확인
10. Pod 생성 및 할당 알림
11. API Server는 etcd에 Pod 생성

## 쿠버네티스 오브젝트

#### Pod

- 가장 작은 배포 단위

**특장**

- 전체 클러스터에서 고유한 IP 할당
- 여러개의 컨테이너가 하나의 Pod에 속할 수도 있음

#### ReplicaSet

- 여러개의 Pod 관리
- 새로운 Pod은 Template을 참고하여 생성
- 신규 Pod을 생성하거나 기존 Pod을 제거하여 원하는 수(Replicas)를 유지

#### Deployment

- 배포 버전을 관리
- 내부적으로 ReplicaSet 이용 - v1, v2

## Service

#### ClusterIP

- 클러스터 내부에서 사용하는 프록시
- pod은 동적이지만 서비스는 고유 IP를 가짐
- 클러스터 내부에서 서비스 연결은 DNS 이용

#### NodePort

- 노드에 노출되어 외부에서 접근 가능한 서비스
- 모든 노드에 동일한 포트로 생성

#### LoadBalancer

- 하나의 IP 주소 외부에 노출

#### Ingress
- 도메인 또는 경로별 라우팅
    - Nginx, HAProxy, ALB

#### 쿠버네티스 API 호출

- 원하는 상태(desired state)를 다양한 오브젝트로 정의하고 API 서버에 yaml 형식으로 전달