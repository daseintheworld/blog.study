---
layout: default
title: "• lambda&DDD2(박진경)"
parent: "architecture"
grand_parent: "study(방법론)"
nav_order: 2
---

### Lambda에서 Lambda를 호출하는 방법

-  동기 invoke
  > 1. 호출할 lambda fuction 등록 (Function-Name) 따기)
  > 2. `aws-sdk`를 설치 (local 실행시에만 필요)
  > 3. name으로 invoke
  'use strict'
const aws = require('aws-sdk');
const lambda = new aws.Lambda({
 region: 'ap-northeast-1' //change to your region`
});
const event = { id: "1", name:"Luna"};
lambda.invoke({
  FunctionName: 'echoTest',
  Payload: JSON.stringify(event, null, 2) // pass params
}, function(error, data) {
  if (error) {
    console.info(error);
  } else {
    console.info(data);
  }
});

- 비동기 invokde
  > 다른부분은 동일하며 InvocationType을 추가적으로 지정하면 비동기 방식으로 호출된다.
lambda.invoke({
  FunctionName: 'echoTest',
  InvocationType: 'Event',
  Payload: JSON.stringify(event, null, 2) // pass params
}

- 참고 : 간단한 lambda to lambda tutorial
[https://www.oreilly.com/learning/how-do-i-invoke-a-lambda-from-another-lambda-in-aws](https://www.oreilly.com/learning/how-do-i-invoke-a-lambda-from-another-lambda-in-aws)

- 장점
-- 로직을 분리할수 있다.

- 단점
-- 비용적인 측면에서 손해가 될 가능성이 크다.
 (Lambda의 과금은 메모리 * 수행시간인데 2번으로 나눠 호출할 경우에는 수행 시간이 길어질수 있으므로)

### Monolithic VS Single-purposed

Monolithic Architecture : 기존에 사용하던 방식. 모든 로직이 프로젝트 하나로 묶여 시간이 지날수록 복잡해지기 쉽다.
  단점 : scale up, 어디가 오류인지 찾기 힘들다. 빌드시간의 장기화, 단일 언어에 종속됨, 선택적 확장 불가, 하나의 서비스를 빌드하기 위해 모든 서비스를 올려야한다.
Single-purposed : 하나의 로직을 위해 개별의 api와 lambda function을 만드는 방식.
   단점 : 복잡한 로직일수록 lambda function이 많이 필요하다. function이 많아질수록 관리하기 괴로워짐.
   장점 : 선택적 확장 가능, 어디가 오류인지 쉽게 찾을수 있다. 개별 서비스 빌드 가능.