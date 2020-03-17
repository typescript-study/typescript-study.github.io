---
title: Accordion FE 1차 Prototype
toc: true
date: 2020-03-17 12:39:05
tags: 
    - Accordion
categories: 
    - Accordion
---

# Accordion Front End 1차 Prototype

## 기존 -> 변경 요점

 `smartAdmin4` 템플릿을 사용하기로 결정

∵ 코드 최적화 및 기술 스택 변경은 가능하지만, 디자인적 능력은 없기 때문에 기존 아코디언 디자인과 가장 비슷한 템플릿 사용

### 기존

기존 템플릿 엔진으로 [handlebars](https://handlebarsjs.com/)를 사용, 렌더링할 때마다 전체 DOM이 항상 업데이트 되서 불필요한 렌더링 발생 - 각 URL마다 `HTML`파일을 만들어 불러 사용함

### 변경 및 개선사항

>  현재 상황과 비슷한 다른 회사의 케이스([NHN 스택 전환](https://meetup.toast.com/posts/220))를 참고하여 작성

1. `vue component`  또는 `web component` 도입 - ∵  `smartAdmin4` 또한 템플릿 엔진이 `handlebars`를 사용하고 있었고, 불필요 렌더링이 존재

   ```html
   # 기존 HTML 코드 방식
   <div class="panel">
     <div class="panel-hdr text-primary">
       <h1>
         제목
       </h1>
       <div class="panel-toolbar"></div>
     </div>
     <div class="panel-container collapse show">					
       <div class="panel-content">
         내용
       </div>
     </div>
   </div>
   
   # 웹 컴포넌트 코드 방식
   <basic-panel width="500px" height="500px">
       <h1>제목 입력</h1>
       <p>내용 입력</p>
   </basic-panel>
   
   # 더 명시적이고, 복잡한 엘리먼트일 수록, 재사용이 쉬워짐
   # 많이 사용되는 엘리먼트일수록 많은 양의 HTML 코드 중복을 피할 수 있음
   ```

2. 타입스크립트 도입

3. 웹팩 사용 (기존 smartAdmin4는 `gulp`를 사용하고 있었음)

   기존 아코디언에서 수많은 라이브러리(CDN) 삽입의 혼란을 방지하고 하나의 `JS` 파일로 합쳐 관리(번들링)

   ▼ 아래 화면 참고

|                             기존                             |                             개선                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| ![기존](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200316135641907.png) | ![개선](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200316135935043.png) |



4. `SPA` 렌더링 - 클라이언트 사이드 렌더링

   하나의 `HTML` 파일을 이용하고 라우팅을 자바스크립트로 조작함

   초기에 전부 로딩을 하여, 이후 빠른 화면전환, 반응성을 가짐

   아코디언은 검색엔진에 노출될 필요성이 없어 서버 사이드 렌더링을 사용하지 않음

5. `scss` 및 `postcss` 도입

   중복되는 CSS 및 구 브라우저 환경에 대응하기 위해 도입
   
   

## Production

### 메인페이지

#### 기존 아코디언 메인페이지

![기본 메인 페이지](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200317102642165.png)

![2](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200317102716776.png)

##### CPU/ Memory/ Network 차트

기존 차트에서 실시간 차트로 변경 (+ 스크롤, 드래그를 이용하여 특정 시간대 확인)

> 현재는 3초마다 랜덤값으로 차트가 표시되어, 난잡할 수 있음

|                             기존                             |                        2. 로그인 하기                        |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| ![기존](https://user-images.githubusercontent.com/26294469/74116605-af672800-4bf7-11ea-9aa5-f7d01670be79.png) | ![개선](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200317093347398.png) |

|                             기존                             |                             변경                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| ![기존](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200317094135965.png) | ![개선](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200317093955695.png) |

##### Pod/ Project/ Service/ Nodes 버튼

기존 아코디언에서 Pods의 개수 및 Running 상태를 알려주는 기능

하지만, 클릭을 하면 팝업이 뜨는 기능이 있음에도 불구하고 누르면 기능이 있을 것처럼 생기지 않아, 
Hover 애니메이션 추가

쿠버네티스 관련 아이콘의 부재로, `pod`, `node`, `volume` 등 SVG 아이콘을 조작 가능한 아이콘 엘리먼트로 변경해야할 필요성 존재 ([SVG 링크 참고](https://github.com/kubernetes/community/tree/master/icons/svg/resources/labeled))

> 이 부분은 조금 더 수정 보완할 생각
>
> 현재는 하나의 버튼이지만, Succee, Fail 별도로 클릭할 수 있게 변경 고려

|                             기존                             |                             변경                             |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| ![기존](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200317094343669.png) | ![개선](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200317094440833.png) |

##### 서비스/ Node 테이블 

> 아직 퍼블리싱(CSS)은 하지않은 상태, 기존 아코디언에 맞춰 보라색 계열로 할 계획
>
> 현재는 더미 API를 통해 데이터를 받아오는 상태

클릭 이벤트가 없고, 보여기만하는 기능으로, 찾는 것이 중요하다고 여겨 `search` 기능 추가

이름 | 디자인 이미지
--------- | ---------
기존 | ![image-20200317095308738](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200317095308738.png) 
개선 | ![image-20200317095229809](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200317095229809.png) 

### Route

![Route](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200317101819139.png)

현재는 페이지 레이아웃 작업이 아닌 UI Element (차트, 버튼, 사이드바, 상단바 등)에 집중하고 있음

> 현재 작업된 페이지는 `/:clusterName/:projectName/dashboard` 와 `:/clusterName/:projectName/monitoring/system 및 atm` 

∵ 페이지마다 재사용되는 엘리먼트들이 많아서, 차트부터 시작해서 기본적으로 사용되는 엘리먼트들 먼저 작업하고 페이지를 하나씩 만들어갈 계획

> 현재는 작업 페이지에 맞춰, 필요한 엘리먼트를 만들어 삽입 중

### 차트

#### 기존 아코디언 차트 모음

> 자세한 사항은 [이 링크](https://bitbucket.org/whitekim88/accordion-frontend/wiki/Accordion_Chart) 를 참고

| ![image-20200317103123352](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200317103123352.png) | ![image-20200317103134344](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200317103134344.png) |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| ![image-20200317103153978](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200317103153978.png) | ![image-20200317103207254](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200317103207254.png) |

#### 현재 이용하고 있는 차트 라이브러리

`chart.js` - [해당 사이트 데모](https://www.chartjs.org/samples/latest/)를 보고 만드려는 차트를 선정

> 또는 특정 이미지, 사이트 URL을 통해 이렇게 만들면 좋겠다는 피드백이 좋을 것 같습니다.

### 사이드바

#### PR#5 🔀 실시간 차트 + 사이드바 구현

![img](https://user-images.githubusercontent.com/26294469/76501257-49cfbb00-6485-11ea-838e-69a4a6124486.gif)



> 프로젝트 개수가 100개 넘을 수 있다는 피드백을 듣고 아래의 기능을 추가

#### PR#7 🔀 사이드바 검색 및 필터링 기능 추가 

##### 검색

![img](https://user-images.githubusercontent.com/26294469/76718580-93b4eb80-677a-11ea-99cc-125d89e4eacc.gif)

##### 필터링

![img](https://user-images.githubusercontent.com/26294469/76718582-957eaf00-677a-11ea-892e-30341254a7b3.gif)

### 상단 링크바

#### PR#11 🔀 상단바 - 모니터링 하위메뉴 추가

![img](https://user-images.githubusercontent.com/26294469/76810143-688cd380-6830-11ea-8767-7bfe9e397dae.gif)

## Development

### Accordion UI Element 및 Docs 정리

왼쪽 패널: 사용할 UI 엘리먼트 정리

가운데 패널: `DEMO`

오른쪽 패널: `DOCS` 및 각 테스트(추가 예정)

![Accordion Storybook](https://raw.githubusercontent.com/taeuk-gang/save-image-repo/image/img/image-20200316130935033.png)

### 테스트

현재 개발 진행에 힘써, 테스트 코드 템플릿만 만들어두고, 테스트 코드 작성이 안되있습니다.

> 개발 일정에 시간적 여유가 있다면, 테스트 코드 작성하는데 일정을 사용하고 싶습니다.

## 개발 진행상황

### 🚧 진행 이슈 (4)

#### [#32: 📝 Accordion Front End 1차 Prototype 작성](https://bitbucket.org/whitekim88/accordion-frontend/issues/32/accordion-front-end-1-prototype)

#### [#28: 🆕 모니터링 > System UI 구현](https://bitbucket.org/whitekim88/accordion-frontend/issues/28/system-ui)

#### [#25: 🆕 레이아웃 초기화 버튼](https://bitbucket.org/whitekim88/accordion-frontend/issues/25/--------------)

#### [#16: 🆕 Default(보라색)/ White/ Dark Theme 선택 가능하게 하기](https://bitbucket.org/whitekim88/accordion-frontend/issues/16/default-white-dark-theme)

#### [#15: 🆕 메인 페이지 - 로컬스토리지를 이용하여, 패널 위치, 크기 저장하여 관리](https://bitbucket.org/whitekim88/accordion-frontend/issues/15/------------------------------------------)



----------

### 📦 준비 이슈 (2)

- 크로스 브라우징(브라우저 / OS별) CI 구축
- keycloak Login 화면 준비



----------



### ✅ 완료 이슈 (31)

#### [#36: 🐛 작은화면에서 breadcrumb 깨지는 현상](https://bitbucket.org/whitekim88/accordion-frontend/issues/36/breadcrumb)

#### [#35: 🐛 사이드바 overflow 처리 못한 버그](https://bitbucket.org/whitekim88/accordion-frontend/issues/35/overflow)

#### [#34: 🆕 모니터링 > APM UI 구현](https://bitbucket.org/whitekim88/accordion-frontend/issues/34/apm-ui)

#### [#33: 🆕 상단의 링크바 구현](https://bitbucket.org/whitekim88/accordion-frontend/issues/33/-------------)

#### [#31: 🐛 touch-start 이벤트 Violation 경고 해결](https://bitbucket.org/whitekim88/accordion-frontend/issues/31/touch-start-violation)

#### [#30: 🆙 사이드바 Search 및 필터 기능 추가](https://bitbucket.org/whitekim88/accordion-frontend/issues/30/search)

#### [#29: 🐛 작은 화면일 때, 사이드바 이상](https://bitbucket.org/whitekim88/accordion-frontend/issues/29/--------------------)

#### [#27: 🆕 GPU 차트 구현](https://bitbucket.org/whitekim88/accordion-frontend/issues/27/gpu)

#### [#26: 🐛 vue-router children 이슈 - 페이지 리로드시 접속 안되는 문제](https://bitbucket.org/whitekim88/accordion-frontend/issues/26/vue-router-children)

#### [#24: 🐛 메인 페이지 - FHD(1920 x 1080) 환경에서 차트 깨짐](https://bitbucket.org/whitekim88/accordion-frontend/issues/24/fhd-1920-x-1080)

#### [#23: 🆕 사이드바 구현](https://bitbucket.org/whitekim88/accordion-frontend/issues/23/----------)

#### [#22: 🔧 views/DashboardView, DraggableTable + store/PageMain 리팩토링](https://bitbucket.org/whitekim88/accordion-frontend/issues/22/views-dashboardview-draggabletable-store)

#### [#21: 🐛 패널 좁게 했을 때, 그래프 깨지는 현상 발생](https://bitbucket.org/whitekim88/accordion-frontend/issues/21/----------------------------)

#### [#20: 🔧 Eslint 설정 (아직 초기 작업 단계라서, 설정 변경이 잦음)](https://bitbucket.org/whitekim88/accordion-frontend/issues/20/eslint)

#### [#19: 🔧 Code Split 필요함 (Vue-Router)](https://bitbucket.org/whitekim88/accordion-frontend/issues/19/code-split-vue-router)

#### [#18: 🆕 UI 컴포넌트 - 메인보드에 사용할 차트 컴포넌트 생성](https://bitbucket.org/whitekim88/accordion-frontend/issues/18/ui)

#### [#17: 🆕 사이드바 마지막 상태 저장하여 관리하기](https://bitbucket.org/whitekim88/accordion-frontend/issues/17/------------------------)

#### [#14: 🆕 PR 검토자들이 테스트할 수 있도록 만들기](https://bitbucket.org/whitekim88/accordion-frontend/issues/14/pr)

#### [#13: 🐛  X버튼 누를때,   레이아웃과 함께 삭제 안됨](https://bitbucket.org/whitekim88/accordion-frontend/issues/13/x)

#### [#12: 🆕 Vuex 생성 (Global 상태 관리기 만들기)](https://bitbucket.org/whitekim88/accordion-frontend/issues/12/vuex-global)

#### [#11: 🆕 CSS 특정 이름으로 네이밍하여, shadow dom 이용시 css 연결](https://bitbucket.org/whitekim88/accordion-frontend/issues/11/css-shadow-dom-css)

#### [#10: 🆕 테스트 코드 템플릿](https://bitbucket.org/whitekim88/accordion-frontend/issues/10/--------------)

#### [#9: 🐛 IE 브라우저 호환 안됨](https://bitbucket.org/whitekim88/accordion-frontend/issues/9/ie)

#### [#8: 🆕 Lit-Element 라이브러리 삽입](https://bitbucket.org/whitekim88/accordion-frontend/issues/8/lit-element)

#### [#7: 🐛 크롬 폰트 흐려짐 이슈 해결](https://bitbucket.org/whitekim88/accordion-frontend/issues/7/------------------)

#### [#5: 🆕 메인 페이지 레이아웃 구현](https://bitbucket.org/whitekim88/accordion-frontend/issues/5/-----------------)

#### [#4: 🙏 eslint 및 prettier 설정 필요](https://bitbucket.org/whitekim88/accordion-frontend/issues/4/eslint-prettier)

#### [#3: 🆕 Vue Component 사용 DEMO 및 DOCS 구현](https://bitbucket.org/whitekim88/accordion-frontend/issues/3/vue-component-demo-docs)

#### [#2: 🔧 현재 .vue, .js 파일을 .ts 로 개선](https://bitbucket.org/whitekim88/accordion-frontend/issues/2/vue-js-ts)

#### [#1: 🐛 Typescript로 바뀌면서, Storybook webpack.config.js 수정 필요](https://bitbucket.org/whitekim88/accordion-frontend/issues/1/typescript-storybook-webpackconfigjs)



