---
layout: default
title: "ZD deployment1(김승용)"
parent: "cloud"
grand_parent: "background"
nav_order: 2
# permalink: "docs/background/backend/dependency-injection"
---

## Zero Downtime Deployment(무중단 배포) 소개
## 배포 전략 : Rolling, Blue/Green, Canary

- - - -

### PPT Link

[](https://docs.google.com/presentation/d/1-hMyEEucn0WmONVExlGj7aqlWFBdBegq3wGvoNJoL74/edit?usp=sharing)

------

### Rolling(In-Place)

현재 가동중인 서버 그대로 한 대씩 구버전에서 새버전으로 교체

![](https://t1.daumcdn.net/cfile/tistory/99143F435C87397A09)

* 서버 인스턴스가 줄어들기 떄문에 트래픽 부담이 생긴다.
* 서버 부하가 걸리지 않을 시간에 배포를 진행하는 것이 좋다.
* 야근과 휴일 근무의 가능성이 있는 방법(?!)

------

### Blue/Green

구 버전과 신 버전을 나란히 구성하고 배포 시점에서 트래픽을 일제히 전환
구 버전은 롤백 서버로 활용가능. 빠른 롤백이 가능.
시스템 지원이 두 배가 필요
![](https://t1.daumcdn.net/cfile/tistory/99A68F4E5C8739470D)

* long-term transaction이 이루어지고 있는 경우
-> 트랜잭션을 양쪽으로 보내는 설계
-> 운영환경 전환 전에 읽기 전용모드, 전환 후에는 읽기-쓰기 모드로 바꾸는 방법 등
* v2의 database migration에 따른 롤백은 어떻게
-> Parellel Change (Expand and Contract Pattern) 사용
https://martinfowler.com/bliki/ParallelChange.html
![](https://martinfowler.com/bliki/images/parallelChange/expand.png)
![](https://martinfowler.com/bliki/images/parallelChange/migrate.png)
![](https://martinfowler.com/bliki/images/parallelChange/contract.png)

DB 적용
예를 들어 컬럼 이름을 바꿀 때,

1. 기존 컬럼 이름을 바꾸지 않고 새로운 컬럼을 만듬
2. 기존 컬럼에서 새 컬럼으로 데이터 마이그
3. 배포에 문제가 없으면 기존 컬럼 삭제

------

### Canary

![](https://t1.daumcdn.net/cfile/tistory/99E6E74C5C8737EC0E)

Canary는 일산화탄소 및 유독가스에 민감한 새
광산에서 위험 알림용으로 사용
위험을 빠르게 감지할 수 있는 배포 기법

구 버전과 신 버전을 구성해서 일부 트래픽을 신버전으로 분산하여 오류여부 테스트
이 역시 DB 마이그를 할 때 Parallel Change Pattern을 사용



> 참고
>
> [이미치 출처](https://onlywis.tistory.com/10)
>
> [ParallelChange(Martin Fawler)](https://martinfowler.com/bliki/ParallelChange.html)
>
> [DevOps : Blue-Green 배포, A/B Testing 그리고 Canary Releases](https://jason-lim.tistory.com/3)
>
> [Blue/Green Deployments with Amazon Elastic Container Service](https://aws.amazon.com/ko/blogs/compute/bluegreen-deployments-with-amazon-ecs/)