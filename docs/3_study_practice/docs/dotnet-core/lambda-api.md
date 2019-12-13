---
layout: default
title: "• solid programming"
grand_parent: "study(실용)"
parent: ".net core"
nav_order: 1
has_children: true
---

### **문서 목적**

* .net core backend에서 에러나 버그와 같은 예측 외의 일을 최소한으로 만드는 방법들 소개


## 각 '관절'의 인식

어떤 책임이 "cloud HIS는 지금껏 경험한 중 가장 답답한 프로젝트다. 한번 개발할 때 체크할 것이 너무 많다"라고 표현했다. 하지만 만약 해야할 것이 많다고 해도 일의 순서가 정리가 되고 그것을 따랐을 때 예측대로 어플리케이션이 움직인다면 불만이 생기지는 않을 것이다.

* 보통 하나의 scope를 통제할 수 있고, 거기서 정의된 기능을 체크하고 싶을 때는 unit test를 사용.
* scope와 scope 사이의, 즉 기능이나 개념이 분리된 상황에서는 각 상황에 맞는 library가 있음.

* **xUnit (Unit Test)** : 자동화 가능한 테스트
* **null check (c# 8.0)** : 지겨운 Object reference not set to an instance of an object
* **automapper test** : loosely coupled 된 내부 시스템의 완성
* **Fluent Validation** : api layer와의 coupling
* **ef core configuration** : data layer와의 coupling

### 1. **xUnit (Unit Test)**
### 2. **null check (c# 8.0)**
operator
https://csharp.today/c-6-features-null-conditional-and-and-null-coalescing-operators/
debugger step through, nullguard
https://stackoverflow.com/questions/29184887/best-way-to-check-for-null-parameters-guard-clauses
### 3. **automapper test**
### 4. **Fluent Validation**
### 5. **ef core configuration**