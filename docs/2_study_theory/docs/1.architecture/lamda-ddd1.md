---
layout: default
title: "• lambda&DDD1(박진경)"
parent: "architecture"
grand_parent: "study(방법론)"
nav_order: 1
---

## Lambda in DDD Architecture
### Unknown Global

**도메인이란**:
도메인의 사전적 의미는 "정보와 활동의 영역" 을 말하며, 흔히 프로그래머들에게는 어플리케이션 내의 로직들이 관여하는 정보와 활동의 영역이라고 받아들여집니다.

**DDD(Domain Driven Desing) 이란**:
도메인 주도 디자인이란 개발을 함에 있어 위에서 설명한 도메인이 중심이 되는 개발 방식을 말하며, 그 목적은 소프트웨어의 연관된 부분들을 연결하여 계속 해서 진화하는 새로운 모델을 만들어 나가 복잡한 어플리케이션을 만드는 것을 쉽게 해 주는 것에 있습니다.
DDD의 핵심적인 목표는 Loose Coupling, High Cohesion 으로 각 도메인이 연결성이 적고 높은 정도로 연관되어 보다 가벼운 설계를 위해 탄생하였습니다.

**AWS Lambda**:
AWS Lambda는 서버를 관리하지 않고도 코드를 실행할 수 있게 해주는 컴퓨팅 서비스입니다.

**FaaS**:
FaaS 는 프로젝트를 여러개의 함수로 쪼개서 (혹은 한개의 함수로 만들어서), 매우 거대하고 분산된 컴퓨팅 자원에 여러분이 준비해둔 함수를 등록하고, 이 함수들이 실행되는 횟수 (그리고 실행된 시간) 만큼 비용을 내는 방식을 말합니다.

PaaS(Platform as a Service)(클라우드에서 제공되는 완전한 개발 및 배포 환경) 와의 주요 차이점
서버 시스템에 대해서 신경쓰지 않아도 된다는 점이 PaaS 와 유사하기도 한데요, 가장 중요한 차이점은, PaaS 의 경우엔, 전체 애플리케이션을 배포하며, 일단 어떠한 서버에서 당신의 애플리케이션이 24시간동안 계속 돌아가고 있다는 점 입니다.

반면 FaaS 는, 애플리케이션이 아닌 함수를 배포하며, 계속 실행되고 있는 것이 아닌, 특정 이벤트가 발생 했을 때 실행되며, 실행이 되었다가 작업을 마치면, 혹은 최대 타임아웃 시간을 지나면 종료됩니다.

### serverless ddd CQRS 형 도식
![](https://image.slidesharecdn.com/serverlessddd-170426073620/95/serverless-ddd-37-638.jpg?cb=1493192298)

### 참고
[serverless ddd](https://www.slideshare.net/AsherSterkin/serverless-ddd)