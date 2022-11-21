---
layout: blog
title:  "Difference Between Localhost and 127.0.0.1"
date:   2022-11-21 00:00
category: gdsc-yonsei
---

웹을 공부하는 사람이라면 localhost와 127.0.0.1이 같다는 사실을 기본적으로 알고 있을 것이다. 나도 그랬는데, 파트 세션에서 Mongoose를 이용해서 Node.js와 MongoDB를 연결해보는 실습을 해보고 있었는데, connection 관련 부분에서 다음과 같은 에러가 발생했다. 분명 코드를 올바르게 쳤는데 다른 사람들 랩탑에선 되고 내 랩탑에선 돌아가질 않아서 너무 답답했다 ... ㅠㅠ

> MongooseServerSelectionError: connect ECONNREFUSED 127.0.0.1:27017

에러 해결을 위해 열심히 구글링을 하다가 [여기](https://www.mongodb.com/community/forums/t/mongooseserverselectionerror-connect-econnrefused-127-0-0-1-27017/123421)에서 해결책을 찾았는데, localhost라고 적혀있는 url을 127.0.0.1으로 바꾸라는 어이없는 답변이었고 더 어이없는 점은 올바른 해결책이었다는 것이다... 그래서 왜 이것이 해결책이 되는지 조사해보기로 했다.

이유는 localhost가 항상 127.0.0.1인 것이 아니기 때문이다. 흔히들 ip 주소로 4개의 숫자 사이 .이 찍인 형태를 생각하는데, 이는 ipv4 형태의 ip 주소 체계이고 주소 고갈 문제를 해결하기 위해 ipv6이라는 새로운 ip 주소 체계가 개발되었다. 그래서 오늘날에는 두 가지가 함께 사용되고 있으며 ipv6에서 localhost는 ::1로 표현된다. 따라서 컴퓨터는 상황에 따라 localhost에 대한 값으로 127.0.0.1을 쓸 때도, ::1을 쓸 때도 있는 것이다.

현재 자신의 컴퓨터가 localhost의 기본값으로 ipv4 address를 사용하는지 ipv6 address를 사용하는지 알고 싶다면 간단히 cmd에 ping localhost를 입력해보면 된다. 만약 Reply from ::1: time<1ms 이런 출력이 나온다면 ipv6 address를 기본값으로 사용하는 것이고, 아니라면 ipv4 address를 사용하는 것이다. 내 랩탑은 ipv6을 기본값으로 사용하고 있었고 localhost가 ::1로 대응되어 127.0.0.1 기준으로 작성된 mongoose 및 MongoDB의 소스코드와 충돌을 일으켜서 발생한 문제였다.

구글링을 통해 알아본 결과 Spring, PostgreSQL 등에서도 같은 문제가 발생하는 듯하고 이러한 문제로 인해 Spring 프레임워크의 실제 소스코드에서도 localhost라고 쓰지 않고 127.0.0.1이라고 주소 자체를 명시하는 컨벤션이 있었다. 프레임워크 소스코드에서도 사용되는 컨벤션인 만큼 나도 localhost를 (const화하여) 127.0.0.1로 코딩하는 습관을 들여야겠다.