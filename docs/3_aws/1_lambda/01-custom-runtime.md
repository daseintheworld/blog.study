---
layout: default
title: "1. custom runtime"
parent: "lambda"
grand_parent: "aws (serverless)"
nav_order: 1
---

[aws link - custom aws lambda runtimes](https://docs.aws.amazon.com/lambda/latest/dg/runtimes-custom.html)

## runtime

* 정의: lambda가 동작하도록 돕는 프로그램. bootstrap이라는 이름으로 배포 패키지 안에 넣을 수 있음.
* 동작:
-- setup code 실행
-- environment 변수로부터 hanlder 이름 추출
-- lambda runtime API로부터 event 수신
-- event로부터 받은 데이터를 handler로 전달
-- handler의 결과를 lambda에 돌려줌.


### bootstrap code의 예시

#!/bin/sh
cd $LAMBDA_TASK_ROOT
./node-v11.1.0-linux-x64/bin/node runtime.js

