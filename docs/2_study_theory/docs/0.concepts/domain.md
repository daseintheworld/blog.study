---
layout: default
title: "• DDD - Domain"
grand_parent: "study(방법론)"
parent: "concepts"
nav_order: 1
has_children: true

---


## Domain?

[wiki에 따르면](https://en.wikipedia.org/wiki/Domain-driven_design)

번역해보면, 특정한 범위의 (1)지식과 (2)영향 혹은 (3)행동.

지식이라는 어휘는 익숙하나,
<br>행동과 특히 영향?이라는 표현은 어색하다.
<br>그러다보니 우리는 항상 domain에 대해 애매모호한 관념을 가지고 있다.
<br>
<br>
혹시 **~~꼰대~~** 상사들이 말하는 '업무'가 바로 domain인가?
<br>
<br>
<br>
**일단 쉬운 개념부터 한 단계씩 다시 가보자.
<br>
<br>
<br>
![](https://i.imgur.com/FjD3VYA.png){:height="400px" width="300px"}
<br><br><br><br><br>

### **1.Domain Expert는?**
<br>

소프트웨어 **외의** 영역의 전문가인 사람들 ==> 온갖 전문가가(전문직 X) 모두 domain expert가 될 수있다
<br>
<br>
개발자로서 테이블 반대쪽에 앉을 사람을 상상하면 되며,
<br>
<br>그런 관점에서 비교적 아주 심플한 정의
<br><br><br><br><br><br><br>


### **2.그래서, 무엇이 문제?**
<br><br><br>
![](http://blog.cnezsoft.com/file.php?f=201711/f_cf5f7291d9f688defb42756ebc8e1343&t=jpg&o=&s=&v=1509945937)
<br><br><br>
문제는 *Communication*
<br><br>우리는 똥을 쌌는데
<br><br>제품 타이틀은 **"한국 최초 클라우드 환경 기반으로 만든 - 매우 싸고 유능한 - 병원 정보 시스템"**
<br><br>고로 여기까지는 이해와 공감이 간다.
<br><br><br><br><br>

### **3. 다시, 왜 domain을 정의해야 하는가?**
<br>
domain driven design은 domain model을 이용하여 문제를 정의하도록 돕기 때문이다.
<br><br>그리고 **domain model**이란
<br><br>context(언어적 맥락), domain, model, **Ubiquitous language**로 구성되어있는 것이다
<br><br><br>
## ??????
<br><br>
...그 와중에 Ubiquitous language의 존재감이 심상찮다
<br><br>**5.소통이 중요하다면서 왜 영어(이왕이면 한국어)가 아니고 ubiquitous language인가?**
<br><br><br><br><br>

### **5.Ubiquitous language**
<br>
[martin fowler 성님 설명](https://martinfowler.com/bliki/UbiquitousLanguage.html)
<br>
<br>
## **"응 Eric Evans가 걍 만든 말"**
<br>
<br>
**martin fawler 왈:**
<br><br>
![](https://martinfowler.com/mf.jpg){:height="200px" width="200px"}
<br><br>-- Ubiquitoous language는 Eric Evans가 ddd 설명할 때 만듦
<br><br>-- 매우매우 명료한 표현으로(rigorous) 개발자와 유저가 공유할 수 있음
<br><br>-- 소프트웨어의 Domain Model에 기반해서 써져있어야함
<br><br>-- Eric이 정확히 가라사대 도메인 전문가와 소통하는데 이걸 쓰는게 아주 중요한 테스팅 단계임
<br><br>

<br><br><br>
**Eric Evans 왈:**
<br><br>
![](https://pbs.twimg.com/profile_images/768528083070705666/mIFI3WYo_400x400.jpg){:height="200px" width="200px"}
<br><br>-- 내가 만든 모델로 의사소통이(language)이 제대로 굴러갈때까지 계속 사용하면 좋음.
<br><br>-- 우리가 얻는 모델은 완벽하고 이해가 깔끔하고, 복잡한 개념을 단순한 요소들로 잘 표현함.
<br><br>-- 도메인 전문가는 용어나 구조가 자기네 이해와 다르거나 어색해지지 않도록 노력해야됨.
<br><br>-- 개발자는 모델 구조가 소프트웨어적으로 명확하지 않은지 살펴봐야됨.
<br><br><br><br><br>

### 6. 그런데, 우리가 만든 도메인 모델은?
<br>기범 생각에는 [martin fawler가 말하는 Anemic Domain Model](https://martinfowler.com/bliki/AnemicDomainModel.html)에 매우 가깝다. 배다른 형제 수준.
<br><br>
anemic: 빈혈
<br><br>
* 일종의 anti-pattern이다.
* 심지어 최근엔 점점 더 많아지고(popular) 있는 것 같다.
<br><br>
**증상**
<br><br>-- 처음엔 '진짜'로 보인다. 명확한 명사로 이름 붙인 것들이 아주 떼로 있어 보이게 나열돼 있다.
<br><br>-- 막상 그것들 사이에 행위를(behavior) 만들려고 보니 이어줄 게 없다. 그냥 서로 ~~븅신~~ 같이 get, set 한다.
<br><br>-- 어떤 팀은 걍 거기 안에 로직을 넣지 말자고 규칙을 정하기도 한다.
<br><br>-- 대신에 service들이 있어서 이 모델들을 가지고 필요한 일들을 전부 한다.
<br><br>
**문제**
<br><br>-- 이 패턴의 제일 븅신같은(fundamental horror) 점은 기본적인 객체지향 개념에 반한다는 것이다.
<br><br>-- 빈혈 패턴은 말그대로 절차적(procedural) 설계일 뿐이고 (소프트웨어 전문가들이) 그동안 없애려고 시도해오던 걸 다시 되살린 셈.
<br><br>-- 최악인건 이 빈혈 모델이 진짜라고 사람들이 믿어서 객체지향의 장점을 모조리 놓친다는 점이다.
<br><br>-- 또한 모델이 단순하다보니 이들이 해결해야 할 문제들을 외부로 넘겨 비용을 전가하는 문제가 생긴다. 예를 들어 데이터베이스 mapping을 하는걸 생각하면 본질적으로 모든 로직은 기존 transaction script를 쓰는 거나 마찬가지다.
<br><br>-- 도메인은 persistence와 presentation에 대해 책임이 없고, validation, calculation, business rule에만 책임이 있어야 하는데 반대가 된다.
<br><br>
**이정도로 생략
<br><br><br><br><br>
### 7. 해결책?
<br><br>
![](http://www.quickmeme.com/img/0c/0ca99d8cf711d2253fe249dd4f5bb3eebea9420d297c601b6b06b14cc2bef13f.jpg)
