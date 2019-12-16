---
layout: default
title: "• DDD: Aggregate1"
grand_parent: "study(방법론)"
parent: "concepts"
nav_order: 5
has_children: true

---

## Aggregate?
<br><br>
![](https://flowframework.readthedocs.io/en/stable/_images/DomainModel-5.png)
<br><br>
앞의 내용으로부터..
1. DDD를 표현하는 것은 Ubiquitous language
2. Ubiquitous language가 이용하는 domain model,
3. Domain model에서 사용하는 가장 중요한 패턴 중의 하나가 aggregate

<br><br>Bounded context까지는 추상화 되었었다면, domain model부터는 구체적인 방법론과 그 이유가 생김.
<br><br>

## 1. Aggregate에 대한 다양한 설명
### **eric evans - [e-book 꼭 읽기를..](https://books.google.co.kr/books?id=xColAAPGubgC&pg=PA511&lpg=PA511&dq=A+cluster+of+associated+objects+that+are+treated+as+a+unit+for+the+purpose+of+data+changes.&source=bl&ots=qbZEdiVJ5t&sig=ACfU3U2sE4hUCs2sC8ZNdG_K3epyZNQQAQ&hl=en&sa=X&ved=2ahUKEwiT5bHB457mAhUGa94KHVEWAxQQ6AEwBHoECAoQAQ#v=onepage&q=A%20cluster%20of%20associated%20objects%20that%20are%20treated%20as%20a%20unit%20for%20the%20purpose%20of%20data%20changes.&f=false)**
* Aggregate는 Domain-Driven Design의 패턴
<br><br>
* 데이터를 수정할 때 한 단위(unit)로 사용하기 위한 관련된 객체들의 모음
<br><br>
* 외부에서 참조하는 것은 aggregate 내의 한 모델에게만 가능하고 root라고 부른다
<br><br>
* 도메인적으로 일관성이(consistency) 있게하는 규칙은 Aggregate 경계 내에서만 적용된다.
<br><br><br><br>

### **martin fawler - [post 3분따리 글](https://martinfowler.com/bliki/DDD_Aggregate.html)**
* Aggregate은 domain 모델들의 묶음이고 **단위 하나**로 취급된다.
<br><br>
* Aggregate은 하나의 객체를 root로 가지며 내부의 다른 모델들을 관리한다.
<br><br>
* Aggregate 외부에서는 오로지 **root를 통해서만 내부에 접근이 가능**하다.
<br><br><br><br>

### **부가 설명 - [post](https://www.infoq.com/articles/microservices-aggregates-events-cqrs-part-1-richardson/)**
<br><br>
![](https://res.infoq.com/articles/microservices-aggregates-events-cqrs-part-1-richardson/en/resources/figure3.jpg){:height="400px" width="300px"}
<br><br>
** 위 이미지에서 '?'의 의미는?

* aggregate끼리의 참조는 아이디를(primary key)를 이용해서만 한다. 즉, 프로그램 상에서 객체를 직접 바라보지 않고 id만 아는, 느슨한(loosely coupled) 상태 라는 것.
<br><br>
* **(중요)**하나의 트랜잭션은 **단 하나의 aggregate만** 만들거나 수정한다.
<br><br>

** 의문: 그렇다면 aggregate끼리 연관관계 혹은 소통을 어떻게 하는가?
<br><br><br>
### **Event-driven 방식으로 소통하여 그 갭을 메꾼다 : Eventual Consistency**
<br><br><br>

## 2. Consistency 문제의 등장
<br><br>
-- BESTCare2.0은 서비스적으로나 시스템적으로 'whole in one.' 트랜잭션 하나에서 말 그대로 모든 database가 접근이 가능했다.
<br>
-- 위 방법은 익히 알다시피 복잡도를 너무 향상시켜왔고 그래서 DDD 방법론에서는 aggregate이 트랜잭션 단위가 된다.
<br><br><br>
![](https://pbs.twimg.com/media/D0nSD7yX0AAru3t.jpg)
<br><br>
### 위 그림에서 본질적이고 **인식론적인** 모순이 읽혀야 한다.
### MSA 기반, 혹은 더 근본적으로 **'data가 세계의 상태를 나타내는'** 컴퓨터 기술 기반 서비스에서 트랜잭션이 분리된다는 것은 현실의 논리에 맞지 않는 상태가 반드시 생길 수 밖에 없다는 것을 의미한다!
<br><br>
* 의료 HIS 기준에서의 문제 - a 서비스가 오더를 생성하고 얼마 지나야만 다른 b,c,d 서비스의 상태가 '진료를 본 상태' 등등으로 바뀐다.
<br><br>
--> b,c,d 서비스 사용자의 불만이 나타난다면, 이 상황을 어떻게 해결해야 하는가?
<br>
--> 가장 전통적인 대답은 '실시간으로 a서비스를 체크해본다'는 것이며 이 시각은 이미 위 그림에 드러난 세계관을 읽지 못하는 것이다.
<br><br><br>
그리고 당연하게도 현재는 이 전통적인 대답을 하는 사람들이 더 많은 상태. 각각의 독립된 세계(서버)가 서로 다른 시간축에서 활동하며, 우리는 '서로의 세계를 침범 안 시키는 선에서' 최대한 그 간극을 서비스의 형태로 줄일려고 노력하는 것. 그것이 MSA 기반 서비스의 가장 중요한 핵심이라 생각한다.(기범)

