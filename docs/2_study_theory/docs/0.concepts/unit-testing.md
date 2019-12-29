---
layout: default
title: "• DDD: Unit Testing"
grand_parent: "study(방법론)"
parent: "concepts"
nav_order: 8
has_children: true
---

![](https://www.jokejive.com/images/jokejive/6e/6e64eeb048f1322666a7bb22d8314c23.jpeg){:width="500px"}
<br><br>
...우리 솔루션은 testing 없이도 동작한다... 왜?
<br><br>
## 0. Unit Testing 없는 개발
<br>
unit testing이 없는 개발이 왜 문제인가?
<br><br>
unit testing는 족쇄인가? 구원의 손길인가?
<br><br><br><br>
이번 글에서 왜 우리가 unit testing을 **자발적으로** 그리고 **적극적으로** 도입하고 싶어해야만 하는지를 알아보고, 특히 DDD에서 가능한(필요한) 측면을 훑어본다.
<br><br><br>

## 1. Unit Testing
<br>
* **정의** - [위키](https://en.wikipedia.org/wiki/Unit_testing)
<br><br>
Unit test는 소프트웨어 개발자가 어플리케이션의 각 부분이("unit"이라고 알려져있음) 설계에 맞고 의도한 대로 행동하는지 확인하기 위한 **자동화된** 테스트를 의미한다.
<br><br>
-- Procedural한 프로그래밍에서는 unit이 곧 전체 모듈. 그러나 보통 한개의 함수나(function) 프로시저를(procedure) 의미한다.
<br><br>
-- Object-Oriented한 프로그래밍에서는 unit이 class나 interface 전체이며, 그러나 개별 함수(method)일 수도 있다.
<br><br><br><br>
* **Unit testing의 장점** - [출처](http://softwaretestingfundamentals.com/unit-testing/)
<br><br>
![](https://i.pinimg.com/originals/3a/84/fb/3a84fbb51448fda8e391f4f546e8caea.jpg){:width="500px"}
<br><br><br>
-- 코드를 수정/유지보수 할 때 **신뢰성을** 높인다. 어떤 코드를 수정했건 한번씩 테스트를 돌리면 즉시 문제 상황을 찾을 수 있다.
<br><br>
-- 코드가 **재사용 가능**하게된다. unit testing이 된다는 것은 모듈화(modular)가 가능하다는 뜻이다. 그러므로 코드가 더 재사용하기 편해진다.
<br><br>
-- **개발이 빨라진다**. 어떻게? 만약 unit testing이 없다면, 너는 코드를 쓴 후 복잡한 '개발자 테스트'를 할 것이다. (중단점을 찍을 거고, GUI를 띄워서, 몇몇 input을 세팅하고, 모조리 원하는대로 돌길 그저 바라기만 한다 -_-) 그러나 unit testing이 있다면, test를 쓰고 돌려보게 된다. test를 작성하는데는 시간이 걸리지만 그 후 테스트를 실행하는 시간을 보상받게 된다.
<br><br>
-- 문제를 찾아서 고치는 데 쓰는 **비용이 높은 단계에서 (higher level) 할 때보다 훨씬 적게든다**. 확인 테스트나 운영 단계에서 발견된 경우보다 드는 비용이(시간, 노력, 장애, 서로에 대한 혐오감..) 적게 들 수 밖에 없다.
<br><br><br>
* **Unit Testing 팁**
<br><br>
-- 모든 케이스에 대해 만들 필요는 없다. 시스템의 행동(behavior)에 영향을 끼치는 것에 대해 만들면 된다.
<br><br>
-- 개발 환경을 테스트 환경으로부터 격리시켜라
<br><br>
-- 운영 환경에 가까운 테스트 데이터를 사용해라
<br><br>
-- **어떤 문제를 발견했으면, 해결하기 전에 그 문제를 노출하는 테스트를 먼저 작성**해라. 왜냐? (1) 만약 그 문제를 정확히 못 해결했으면 나중에라도 상황을 발견하게 된다. (2) 너의 테스트가 더 (3자에게) 이해가된다. (3) 문제를 이미 고쳤으면 게으른 너는 아마 굳이 테스트를 안 만들 것이다.
<br><br>
-- 행위들을 체크하는 케이스와 더불어서, 퍼포먼스 관련 테스트도 작성해라
<br><br>
-- unit testing은 지속적으로 빈번하게 수행해라.
<br><br><br><br>
## 2. DDD와 Unit Testing - [출처](http://www.taimila.com/blog/ddd-and-testing-strategy/)
<br>
**먼저 구조를 살펴보자**
<br><br>
![](http://www.taimila.com/assets/images/ddd_testing_post/Architecture.png){:width="500px"}
<br><br>
(우리는 오랜 공부를 통해 이제 각 단어를 설명할 수 있어야함 :)
<br><br>
결론적으로 우리는 한개의 어플리케이션 내에서 Domain Layer, Persistency Layer, Application Layer에 대한 unit testing을 수행할 수 있다.
<br><br><br>
* **Domain Layer**의 unit testing
<br><br>
DDD의 도메인 모델은 지속성(persistence)에 대해 ~~ㅈ도~~ 쥐뿔도 관심이 없다. 즉, 저장(save) 메소드가 없다는 뜻이다. 이건 테스팅을 하려는 개발자들에게 행복한 뉴스다. Domain logic 안의 in-memory로서만 테스트가 가능하며 그 밖의 종속성(dependency)가 없다는 뜻이기 때문이다.
<br><br>
**잠깐, persistence는 한국말로 번역하기에 어려운 의미를 담고 있다. 이 [포스팅](https://homo-ware.tistory.com/4)에서 꽤 좋은 설명을 하였다. 이후에는 "지속성"이라고만 쓴다
<br><br>
-- unit testing은 aggregate로!
<br>도메인 모델은 aggregate의 모임이고, unit testing은 당연하게도 각 aggregate에 대해 이루어지면 좋겠다. 짜가 생성이 필요한 건(mocking) 또다른 aggregate에 대해서 뿐이다. 
<br><br>
-- 구체적인 설명이 길게 있지만 생략하겠습니다. [출처](http://www.taimila.com/blog/ddd-and-testing-strategy/)에 가서 꼭 보시길..
<br><br><br>
* **Persistence Layer**의 unit testing
<br><br>
Persistence layer는 저장소(repository)를 구현한 어플리케이션을 포함한다. 저장소(respository)는 도메인 aggregate의 데이터 지속성을 유지시키기 위한 클래스들을 의미한다. Unit testing은 이 구현물들이 정말로 aggregate들을 잘 저장하고 다시 끌고오는지 확인하는 데 있다. 데이터베이스와 ORM 도구들이 여기서 중요한 역할을 한다.
<br><br>
이 테스트들은 in-memory 테스트들보다 느리지만, 괜찮다. 굳이 다른 테스트들처럼 허구헌날 할 필요가 없다. 지속성 관련 로직은 도메인 로직에 비해 수정이 훨씬 덜 일어난다. 
<br><br>
저장소 테스트는 단순하게 도메인 aggregate들을 저장하고, 다시 빼내오고 각 부분이 잘 있는지 확인한다. 이런 종류의 지속성 행동들은 각 저장하는 기술의 디테일에 크게 연연하지 않는다. 그러므로 예를 들어 MongoDB에서 다른 DB로 바꿔도 테스트 케이스가 바뀔 일은 없다. 데이터베이스 스키마가 바뀌어도 테스트 내용은 변함이 없을 것이다.
<br><br><br>
* **Application Layer**의 unit testing
<br><br>
![](http://www.taimila.com/assets/images/ddd_testing_post/Application-Layer.png)
<br><br>
어플리케이션(application) 단위에서는 위 이미지와 같이 비즈니스 기능(functionalities)를 구현한다. 한 클래스(class) 마다 하나의 기능이 있을 때도 있지만, 보통은 많은 케이스가(use-case) 여러 클래스에 섞여들어가며, 여러 aggregate와 저장소에 대해 기능을 수행한다.
 <br><br>
 각 케이스를 수행할 때 여러가지 외부적으로 쓰이는 종속성을 다 짜가로(mocking) 만들어줘야한다.
 <br><br>