---
layout: default
title: "• no downtime2(김승용)"
parent: "deployment"
grand_parent: "study(방법론)"
nav_order: 7
---
# Zero Downtime Deployment with DB Migration
<br>

## Assumption

1. **롤백을 할 때 Application만 롤백을 하고 DB는 롤백하지 않는다.**
2. **롤백을 할 때 하나 전 단계로 롤백을 한다. 두 단계 이상 롤백은 하지 않는다.**

<br>

## Step 1 Initial State

![](https://user-images.githubusercontent.com/23690559/69634215-1cf1c680-1095-11ea-98e7-a389479b93c1.png)

<br>

## Step 2_Bad Backward-incompatible way

###### ![](https://user-images.githubusercontent.com/23690559/69634219-1f542080-1095-11ea-9b58-b8f3c821080e.png)


<br>

## [Rollback] Step 2_Bad => Step 1

![](https://user-images.githubusercontent.com/23690559/69637750-a062e600-109c-11ea-83e0-e1e32f00efdc.png)

App에서는  `lastName`을 찾지만 DB에는 `surname`만 있기 때문에 에러가 발생


<br>

## Step 2 Adding surname

![](https://user-images.githubusercontent.com/23690559/69634225-224f1100-1095-11ea-91f6-8f5479f94a26.png)

데이터를 `last_name`과 `surname`에 저장하는 App 2.0.0을 배포한다. App은 `last_name` Column에서 데이터를 읽는다. DB v2는 `last_name`과 `surname` Column을 모두 가지고 있는다. `surname`은 `last_name`을 복제하여 만든다.( NOT NULL 제약조건이 있으면 안된다. )


<br>

## [Rollback] Step 2 => Step 1

![](https://user-images.githubusercontent.com/23690559/69637755-a48f0380-109c-11ea-8c07-60deda9e6308.png)

롤백을 하더라도 DB에 `last_name` 이 남아 있으므로 에러가 발생하지 않는다. 정상적인 데이터 CRUD가 가능하다.


<br>

## Step 3 Removing last name from code

![](https://user-images.githubusercontent.com/23690559/69634231-25e29800-1095-11ea-987c-f1519bbbf223.png)

데이터를 `surname`에만 저장하고 `surname`에서만 읽는 App 3.0.0을 배포한다. `last_name`에서 `surname`으로의 최종 마이그레이션이 일어난다. `last_name`의 NOT NULL 제약조건을 드랍한다. DB 버전은 v3.


<br>

## [Rollback] Step 3 => Step 2

![](https://user-images.githubusercontent.com/23690559/69637764-a6f15d80-109c-11ea-99a6-6ddac78448c2.png)

APP의 메소드 `getSurName()`, `setSurName()`은 Read 때 `surname`에서 우선 가져오고 null일 때는 `last_name`에서 가져오고, 데이터를 입력할 때 Write 때 `surname`과 `last_name`에 모두 입력하기 때문에 문제될 것이 없다.


<br>

## Step 4

![](https://user-images.githubusercontent.com/23690559/69634234-28dd8880-1095-11ea-913f-c7f2976596cc.png)

버전 4.0.0을 배포한다. 코드에는 변화가 없다. `last_name`에서 `surname`으로의 최종 마이그가 수행됐고, `last_name`이 삭제된 DB v4를 배포한다. 적용하지 않았던 제약조건을 적용한다.


<br>

## [Rollback] Step 4 => Step 3

![](https://user-images.githubusercontent.com/23690559/69637771-aa84e480-109c-11ea-9101-04b176a99e45.png)

Application단에서 last_name을 아예 참조하지 않기 때문에 문제되지 않는다.