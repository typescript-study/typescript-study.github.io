---
title: Accordion FE 2차 Prototype 준비사항
toc: false
date: 2020-03-18 12:39:05
tags: 
    - Accordion
categories: 
    - Accordion
---

## Accordion-FE prototype-v2  준비사항

### 요약

예상 일정: ~ 2020. 04 . 03. 금 까지

#### 메인 새기능

1. CRUD 테이블 컴포넌트 구현 - GET/POST/DELETE 대응
2. `/admin/overview` 페이지 구현
3. `/login` 페이지 구현 - `keycloak` 연동
4. 모니터링 대시보드 - Active server chart 구현

#### 부가 새기능

1. REST API 연결 - 현재는 `heml chart`  부분 가능



-------

### 피드백 필요사항

1. 엔지니어분들이 주시는 데이터를 표시하는 표준적인 차트에 대한 지식이 부족 (예시. xlog, ElapsedTime, etc)

기존 아코디언을 제외하고 이를 참고할만한 좋은 예시(링크, 이미지)가 필요

예시. "이 부분은 제니퍼 소프트에 어떤 차트같이 만들었으면 좋겠다."

![제니퍼 소프트](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200318144935073.png)

2. 오픈시프트에서 참고할만 요소(링크, 이미지) 필요 - ∵ 오픈시프트에 대한 지식 부족

### 개선 사항

1. 차트의 `zoom` 기능이 기존 스크롤과 중복되어 발생되는 버그들이 존재하여, 드래그하여 시간을 이동하는 기능만 넣고 시간축 확대 기능은 제외할지 고민 중
2. 메인 페이지 - `Pod, Service, Project, Node` 엘리먼트, Running/ Fail 나누어서 클릭하여 모달창 띄울 예정
3. 현재 SPA 라우팅 속도 문제 해결 필요 - 화면의 FOUS 발생없이 한번에 화면 전환이 되야함

### 기획 사항

1. `프로젝트 - 앱  - 앱 추가` 기능에 현재 테이블 형태가 아닌, 스크롤이 존재하지않고 탭으로 관리하는 형태가 되면 어떨지 고려 중 (▼ 아래 그림은 예시)

![예시](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200318144736298.png)