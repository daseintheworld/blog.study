---
layout: default
title: "• DDD: Domain Model"
grand_parent: "study(방법론)"
parent: "concepts"
nav_order: 2
has_children: true

---

**domain은 정의하기 어렵기 떄문에 여러 시각이 있음.
<br>

## 다른 관점의 Domain Model?

이전 글에 따르면,
<br>context(언어적 맥락), domain, model, **Ubiquitous language**로 구성되어있는 것.
<br><br>
### **다른 관점 Domain**
<br>
[포스팅에 따르면](https://culttt.com/2014/11/12/domain-model-domain-driven-design/)
<br><br>Domain == Business Problem
<br><br>도메인은 우리가 풀려는 문제에 대한 아이디어, 지식, 데이터의 총합이다. 도메인 전문가들은 자신들이 마주한 문제에 대해 해결하기 위한 도구나 목표를 이미 갖추고 있다.
<br><br>그러므로 "Domain Driven Design" == "Business Problem Driven Design"
<br><br><br><br>
### **Model**
<br><br>모델은 **solution**이다.
<br>-- 모델은 현실의 일면이나 관심사를 표현해준다.
<br>-- 모델은 더 큰 개념에 대한 단순화이고 그 결과 solution에 필요한 외 나머지는 무시된다.
<br>-- 그러므로 특정한 문제에 대해 단순하고 구조화된 지식에 집중해야한다.
<br><br><br><br>
### **Domain + Model**
* 도메인 모델은 그 문제에 대한 핵심 컨셉과 어휘로 표현해서 그 안의 entity와 관계를 찾아낸 결과
- 도메인 모델은 도식화(diagram), 코드(code), 문서(document)일 수도 있다.
<br><br> - 중요한 것은 domain model이 프로젝트 내의 어휘로 정의가 되고, **모든 사람이 대화하는 수단**이 되어야한다는 것이다.
<br><br>* Ubiquitous Language가 DDD(domain driven design)에 매우(extremely) 중요한 개념이 된다.
<br><br><br><br>
### **Domain Model이 왜 중요한가**
<br><br>* 설계의 핵심이다 : 우리가 짜는 코드는 그 model에서 바로 출발해야한다.
<br><br>* 모든 팀 멤버가 소통하는 언어의 핵(backbone)이다 : 기술자든 아니든 무조건 Ubiquitous Language를 써야한다.
<br><br>* 증류과정을(distilled) 거친 지식이다 : 어떤 의사결정에든 도메인 모델을 꺼내 대화하는걸 반복적으로(iterative) 거쳐야한다.
<br><br><br><br>
### **어떻게 Domain Model을 찾는가**
<br><br>포스팅은 추상적이다. 우리 스스로 생각
<br><br><br><br>
### **Domain Model의 예시?**
<br><br>다음 문제상황을 보자
1. '프로젝트'를 관리하는 제품을 만들려고한다.
2. '프로젝트'는 '제품의 로드맵'(roadmap)으로부터 나온다.
3. '프로젝트'는 '일정 기준'(criteria)에 의해 선택된다 The project is picked due to certain criteria
4. '프로젝트'는 '프로젝트 관리자'(manager)에게 할당된다.The project is assigned to a project manager
5. '프로젝트 관리자'는 팀을 이끈다The project manager organises the team
5. '이터레이션'(sprint)을 계획한다.
<br><br> * 이 문제는 '비즈니스' 도메인에 관한 것
<br><br> * entity는 Roadmap, Project Manager, Employees, Team, Sprint가 된다.
<br><br> * '프로젝트를 선택'하기 위해 그걸 도메인 로직인 '일정 기준'(criteria)를 찾아내 의사결정 하는 것을 도와야한다.
<br><br> * '이터레이션'(sprint)을 어떻게 계획하는지(planning)에 대해 시간이나 그 외 자원을 정의하는 방법을 정해야한다.
<br><br><br><br>
### **ㅇㅇ**