# 📺 지금 뉴스 - 실시간 뉴스를 한눈에

<p>
뉴스 API를 바탕으로 <b>경제, 정치, 생활, 시사 등의 카테고리별</b>로 나뉘어 출력되게 하였고 키워드로 검색이 가능하게 하여 손쉽게 기사를 검색할 수 있도록 하였습니다. 또한 기사들을 스크랩할 수 있게 하여 <b>스크랩된 기사들을 나만의 뉴스 탭</b>에서 모아볼 수 있도록 하였습니다.
</p>

---------

## 💡 1. 주제
**뉴스 API**를 호출하여 카테고리별, 언론사별로 흩어져있는 기사들을 하나의 앱 화면에 출력해보며 단순히 기사 출력이 아닌 **스크랩 및 검색 기능**을 추가한 간단한 뉴스 앱

## 📱 2. 핵심 기능
- **뉴스 API 기반 실시간 업데이트**: 뉴스 API 기반 뉴스들이 바로바로 업데이트되어 실시간으로 현재의 기사들을 볼 수 있음 
- **카테고리 분류**: 뉴스 기사의 4개의 대표 키워드를 카테고리로 묶어 분류
- **기사 스크랩**: 관심있는 기사를 스크랩하여 나만의 뉴스 보관함에 보관할 수 있는 스크랩 기능 제공
- **키워드 검색**: 키워드 검색 기능을 제공하여 대표 키워드 외의 궁금한 키워드의 기사 검색 가능

## 💻 3. 주요 기술
- **Front-End**: ![Vue.js](https://img.shields.io/badge/Vue.js-4FC08D?style=flat-square&logo=vuedotjs&logoColor=white)
![SCSS](https://img.shields.io/badge/SCSS-CC6699?style=flat-square&logo=sass&logoColor=white)
- **데이터관리**: ![JSON](https://img.shields.io/badge/JSON-000000?style=flat-square\&logo=json\&logoColor=white)
- **환경**: ![VSCode](https://img.shields.io/badge/VSCode-007ACC?style=flat-square\&logo=visualstudiocode\&logoColor=white) ![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat-square\&logo=github\&logoColor=white)
- **디자인**: ![Figma](https://img.shields.io/badge/Figma-F24E1E?style=flat-square\&logo=figma\&logoColor=white)
- **배포**: ![Vercel](https://img.shields.io/badge/Vercel-000000?style=flat-square&logo=vercel&logoColor=white)

------------------

## 🔗 배포 URL
**지금뉴스** https://right-news.vercel.app/

---------------

## 📅 개발 기간 및 인원
* 2026.05 
* 개인 프로젝트

---------

## 🗂️ 폴더 구조

```
📂 src/
┣━━ 📂 assets/
┃   ┗━━ 🖼️ logo.png                  # 이미지 리소스
┣━━ 📂 components/
┃   ┗━━ 📄 HelloWorld.vue            # 공통 컴포넌트
┣━━ 📂 router/
┃   ┗━━ 📄 index.js                  # 페이지 라우팅 설정
┣━━ 📂 store/
┃   ┗━━ 📄 index.js                  # 전역 상태 관리 (Vuex)
┣━━ 📂 views/
┃   ┣━━ 📄 HomeView.vue              # 메인 페이지
┃   ┗━━ 📄 AboutView.vue             # 소개 페이지
┣━━ 📄 App.vue                       # 전체 레이아웃
┗━━ 📄 main.js                       # Vue 앱 진입점    
```

-------

## 💥 트러블슈팅 (Trouble Shooting)

### 📌 1. 기사 링크 동작 제어 및 상세보기 화면 전환
> **❗ 문제**: a태그로 묶여진 기사의 링크를 :href='item.image'로 넣으니 기사의 언론사 페이지가 새창으로 열리는 문제 발생

- **✅ 해결 방안 (Solution)**
  - **클릭 이벤트 제어를 통한 상세보기 구현**: a태그에 @click.prevent를 주어 새창으로 넘어가는 기본 클릭이벤트를 방지하고 새창이 아닌 화면 전환으로 기사 상세보기가 되도록 openDetail함수를 만들어 클릭했을때 기사 상세보기가 되도록 해야하는 img태그와 b태그에 @click.prevent='openDetail(item)'을 넣어 다른 영역의 클릭 이벤트는 막으면서 상세보기 화면 전환이 이뤄지게 함.

---

### 📌 2. 기사 상세보기 진입에 따른 화면 구성 분리
> **❗ 문제**: 기사 상세보기 화면에서도 여전히 상단의 검색창과 카테고리탭이 뜨는 문제 발생

- **✅ 해결 방안 (Solution)**
  - **선택한 기사 데이터 결합 및 조건부 화면 렌더링**: 기본 기사리스트들을 ...item으로 나열해서 기사 상세내용링크을 res로 받은것을 활용해 await res.json()으로 정의된 detail과 함께 this.selectedItem={...item,...detail}로 선택된 기사 selectedItem를 활용함. 그 후 선택된 기사가 있을경우, 즉 디테일에는 v-if='selectedItem로 나오게, 반대로 나오면 안되는 검색창과 카테고리 탭에는 v-if='!selectedItem'을 주어 화면 구성에 차이를 둠.

---

### 📌 3.크롤링 데이터 정제 및 불필요한 HTML 요소 제거
> **❗ 문제**: 웹사이트의 기사를 크롤링해서 가져오다보니 각각의 언론사마다 기사 내용외에 필요없는 정보가 포함되어 있는 문제 발생

- **✅ 해결 방안 (Solution)**
  - **Cheerio를 활용한 불필요한 HTML 요소 제거**: 가져올 내용을 담은 부모 태그들을 담은 selectors중 하나인 selector을 반복문으로 돌릴때 필요없는 태그를 지우기 위해 $(selector).html()로 잡고 브라우저의 document처럼 태그를 잡기 위해 문자열을 const $$=cheerio.load(a)로 객체로 변환하면 $$('script')처럼 해당 태그를 잡을 수 있으므로 뒤에 remove()를 붙여 불필요한 정보가 담긴 해당 태그를 아예 제거함.

---
