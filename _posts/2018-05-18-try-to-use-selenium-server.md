---
layout: post
title: "Selenium Server를 세팅하면서 겪은 시행착오들"
subtitle: ""
date: 2018-05-18
author: ttuyon
category: diary
tags: [angular, e2e-test, protractor, selenium, selenium-server]
finished: true
---

### virtualbox-windows10에서 selenium node서버 설정

- selenium standalone server의 설치 및 실행과 각 브라우저 드라이버를 관리해주는 npm package를 설치하여 실행해보았지만, hub서버가 정상적으로 켜져있음(이라고 생각)에도 연결이 안됐음, 연결은 됐는데 연결이 안됐다고(->???) 에러 발생
- selenium webdriver 홈페이지에서 직접 selenium standalone server jar 파일을 받아서 config 파일과 함께 실행함

➡️ 연결 완료!!!!

- protractor로 e2e 테스트를 시작하면 hub서버에서 node서버에 접속하여 os정보는 가져오는데 IE 드라이버를 찾을 수 없다고 에러 발생, node서버에서는 아무런 반응도 하지 않음(명령 프롬프트에 아무런 반응 없음)

➡️ hub서버가 뭔가 잘못됐나...?

### selenium hub서버 설정

- `node_module`에 있는 `protractor`의 `webdriver-manager`를 통해서 selenium hub서버를 켰음
- 위와 같은 경우 발생
- 마찬가지로 selenium webdriver 홈페이지에서 직접 jar 파일을 받아서 hub서버 실행함

➡️ virtualbox-windows10에서 혼자 IE를 실행시킴!!!!

- protractor로 e2e 테스트를 시작하면 브라우저만 띄우고 테스트가 실행되지 않고 jamine timeout 에러 발생
- 같은 장치 다른 브라우저(chrome)로 테스트 해보면 잘 됨

➡️ 익스플로러가 무슨 문제가 있다

- 보호모드 설정을 통일하라고 해서 했는데도 안됨
- 보호수준까지 통일

➡️ 혼자 input에 타이핑 하며 테스트가 실행됨

- 타이핑 속도가 너무x100 느려서 계속 jasmine timeout 발생
- 32bit용 IE 드라이버로 교체

➡️ 잘된다 🎉🎉🎉
