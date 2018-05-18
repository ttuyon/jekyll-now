---
layout: post
title: "Protractor를 사용하기 위해 겪었던 시행착오들"
subtitle: ""
date: 2018-05-18
author: ttuyon
category: articles
tags: [angular, e2e-test, protractor, selenium]
finished: false
---

### virtualbox-windows에서 selenium node 설정 
- selenium-standalone, 각 브라우저 드라이버를 관리해주는 npm package를 설치하여 실행해보았지만, hub서버가 정상적으로 켜져있음(이라고 생각)에도 연결이 안됐음 (연결은 됐는데 연결이 안됐다고(->???) 에러 발생)
- selenium 홈페이지에서 직접 jar 파일을 받아서 config 파일과 함께 실행함

➡️ 연결 완료!!!!

- e2e 테스트를 시작하면 hub서버에서 드라이버를 찾을 수 없다고 에러 발생

➡️ hub서버가 무언가 잘못됐을수도 있겠다..

### selenium hub 설정
- `node_module`에 있는 `protractor`의 `webdriver-manager`를 통해서 selenium hub 서버를 켰음
- 위와같은 경우 발생
- selenium 홈페이지에서 직접 jar 파일을 받아서 hub 서버 실행함

➡️ virtualbox-windows에서 혼자 익스프로러를 실행시킴!!!!

- e2e 테스트를 시작하면 테스트가 실행되지 않고 jamine timeout 에러 발생
- virtualbox-windows chrome 브라우저로 테스트 해보면 잘 됨

➡️ 익스플로러가 무슨 문제가 있다

- 보호모드 설정을 통일하라고 해서 했는데도 안됨
- 보호수준까지 통일

➡️ 혼자 input에 타이핑 하며 테스트가 실행됨

- 타이핑 속도가 너무x100 느려서 계속 jsamine timeout 발생
- 32bit의 익스프로러 드라이버로 교체

➡️ 잘된다!!!
