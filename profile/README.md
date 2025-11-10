<div align="center">

# 📰 남표뉴스 (Nampyo News)

**진보/보수 언론의 ‘프레임’을 한 눈에 보여주는 뉴스 분석·시각화 서비스**

남민지 · 김선표 (Roflaff)  
BIGKINDS & Naver 뉴스 데이터를 활용한 **키워드 기반 프레이밍 분석 웹 프로젝트**

<!-- 필요하다면 여기에 링크를 추가하세요 -->
<!-- [Web Demo](https://...) · [Paper](https://...) -->

</div>

---

## 🔎 남표뉴스 한 줄 소개

> **“같은 뉴스를 두고, 진보·보수 언론은 어떤 단어로 이야기를 짜는가?”**

남표뉴스는 한국 주요 일간지의 뉴스를 수집한 뒤,

- 언론사를 **진보(blue) / 보수(red)** 로 나누고
- 각 진영에서 자주 쓰는 **키워드를 자동으로 집계**하여
- 그 결과를 **웹에서 직관적으로 시각화**하는 프로젝트입니다.

초기 버전은 **노란봉투법** 보도를 사례로 삼아,
경향·한겨레(진보)와 조선·동아(보수)의 키워드 패턴을 비교하는 연구를 기반으로 만들어졌습니다.

---

## 🧾 Web Abstract (논문 초록 – 웹 버전)

**Abstract**

_(예시: 이 아래 문단을 본인이 작성한 초록으로 교체하세요)_

Recent advancements in data-driven journalism have enabled frame analysis based on large-scale news archives. Nampyo News is a web-based system that collects articles from Korean progressive and conservative newspapers, classifies them into political camps, and visualizes keyword distributions to reveal framing differences. Focusing on the Yellow Envelope Act (노란봉투법) as a case study, our system analyzes 744 articles published over 119 days, extracting noun-based keywords from BIGKINDS data and aggregating them by political camp. The results show that progressive media highlight labor-related concepts such as “노동자”, “원청”, and “교섭”, while conservative media emphasize political and economic actors such as “민주당”, “대표”, and “기업”. By exposing these patterns through an interactive web interface, Nampyo News supports both academic research on media framing and public understanding of how news narratives differ across ideological lines.

> ⚠️ 실제 논문 초록으로 교체할 때는 위 예시 문단을 삭제하고, 자신의 한국어/영문 초록을 그대로 붙여넣으면 됩니다.

---

## 🧩 기능 개요

사용자는 남표뉴스에서 다음과 같은 작업을 할 수 있습니다.

- 🔍 **검색어 + 기간**을 입력해 관련 기사들을 불러오기
- 📰 **언론사/진영(blue/red)** 을 선택해 분석 대상 좁히기
- 📊 각 진영별 **상위 키워드 TOP 10** 및 빈도(%) 비교
- ⏱ **Naver 뉴스 실시간 스트림(SSE)** 으로 최신 기사 흐름 확인

초기 연구에서

- 진보 언론은 **“노동자, 원청, 교섭”** 같은 키워드를,
- 보수 언론은 **“민주당, 대표, 한국, 기업”** 같은 키워드를 상대적으로 더 강조하는 패턴이 확인되었습니다.

이러한 분석 로직을, 사용자가 검색어와 기간만 입력하면 웹에서 그대로 재현해 주는 것이 남표뉴스의 핵심입니다.

---

## 🧱 시스템 구조 한눈에 보기

### Frontend (별도 레포)

- 역할: 검색·필터 UI, 결과 시각화, 기사 리스트/상세 보기
- 설계
  - **Figma**로 주요 화면 설계
  - [**v0.app**](https://v0.app/) 으로 초기 React 구조 생성 후 GitHub로 이관  
  - VS Code의 **Copilot / Simple Browser / Add element to chat** 를 이용해 자연어 기반 “바이브 코딩” 진행
- 레포 (예시)
  - [`nampyo-news/nampyo-news-FE`](https://github.com/nampyo-news/nampyo-news-FE)

### Backend (이 레포)

- 역할: 뉴스 수집, BIGKINDS/엑셀 데이터 분석, 키워드 집계 API
- 스택
  - Python 3.10, FastAPI, Uvicorn
  - httpx (비동기 외부 API)
  - pydantic v2 & pydantic-settings
  - Server-Sent Events (SSE)
  - 파일 기반 로깅 (KST)

- 주요 엔드포인트
  - `GET /`  
    - 헬스 체크 (`{"Hello": "World"}`)
  - `GET /public/v1/news/sse`  
    - Naver 뉴스 검색 결과를 **15초 간격 SSE**로 스트리밍
    - Query: `query` (검색어)
  - `POST /public/v1/news/bigkinds/get_info`  
    - BIGKINDS/엑셀 데이터를 이용해 기사 목록 + 진영별 키워드 TOP 10 반환

---

## 👥 팀 소개 (Team)

| 이름 | 역할 | GitHub |
|------|------|--------|
| **김선표** | 백엔드 개발 · 인프라 · 데이터 파이프라인 | `@Roflaff` (예: https://github.com/Roflaff) |
| **남민지** | 데이터 기획 · 프론트엔드 · 저널리즘 리서치 | `@YOUR_ID` (예: https://github.com/YOUR_ID) |

> 실제 깃헙 아이디/링크로 수정해서 사용하세요.

---

## 🖼 작동 화면 / 환경 스크린샷

README에서 바로 서비스 느낌을 볼 수 있도록 이미지 영역을 미리 잡아둔 섹션입니다.  
`./docs` 폴더에 스크린샷을 추가하고 파일명만 맞춰주면 됩니다.

### 1) 메인 화면 – 분석 결과 대시보드

![메인 화면 예시](./docs/screenshot-main.png)

### 2) 진영별 키워드 비교 뷰

![키워드 비교 예시](./docs/screenshot-keywords.png)

### 3) 실시간 뉴스 스트림 (SSE)

![실시간 뉴스 스트림 예시](./docs/screenshot-sse.png)

> 필요하면 추가로 “실행 환경(로컬 개발 화면, 서버 구조 다이어그램 등)” 이미지를 같은 방식으로 넣으면 됩니다.

---

## ⚙️ 설치 & 실행 (Backend)

### 1. 의존성 설치

```bash
poetry install
````

### 2. 환경 변수 설정 (`.env.local` 예시)

```env
X_NAVER_CLIENT_ID=your_naver_client_id
X_NAVER_CLIENT_SECRET=your_naver_client_secret
BIGKINDS_API_KEY=your_bigkinds_api_key  # BIGKINDS API 사용 가능 시

BLUE_PROVIDERS=["경향신문", "한겨레"]
RED_PROVIDERS=["조선일보", "동아일보"]
```

### 3. 개발 서버 실행

```bash
poetry run uvicorn nampyo_news.main:app --host 0.0.0.0 --port 8000 --reload
```

* API 문서: `http://localhost:8000/docs`

### 4. 간단 SSE 테스트

```bash
python client_test.py
# 검색어를 입력하면 /public/v1/news/sse 스트림 결과가 콘솔에 출력됩니다.
```

---

## 🔌 외부 서비스 연동 요약

### Naver Search API

* 엔드포인트: `GET https://openapi.naver.com/v1/search/news.json`
* 인증:

  * `X-Naver-Client-Id`, `X-Naver-Client-Secret`
* 용도:

  * `/public/v1/news/sse` 에서 **15초 간격으로 최신 뉴스 목록**을 가져오는 데 사용

### BIGKINDS / 엑셀 기반 분석

* 기존: `POST https://tools.kinds.or.kr/search/news` (BIGKINDS API)
* 용도:

  * 기사 메타데이터 + 키워드(TMS) 수집
  * 진보/보수 진영별 키워드 빈도 상위 10개 계산

---

## 🚧 Roadmap (추후 개발 계획)

### 1. BIGKINDS API 사용 불가에 따른 구조 전환

최근 BIGKINDS API 정책 및 접근성 이슈로 인해
**직접적인 API 호출 없이도 동일한 분석 기능을 제공하는 방향**으로 개발을 진행 중입니다.

* BIGKINDS에서 제공하는 **뉴스 엑셀 데이터**를 주기적으로 내려받아 저장
* 백엔드에서 엑셀을 파싱해

  * 기사 목록 및 메타데이터 로딩
  * 키워드(TMS) 컬럼으로 진영별 키워드 빈도 계산
* 프론트엔드는 기존과 동일한 API 스펙을 유지
  → 내부적으로만 **“API 호출 → 엑셀 기반 처리”** 로 변경

> 즉, 사용자는 그대로 남표뉴스를 사용하지만,
> **백엔드 데이터 소스가 “BIGKINDS API → BIGKINDS 엑셀 데이터”로 바뀌는 형태**입니다.

### 2. Naver 실시간 뉴스 기능 확장

* 실시간 스트림으로 받은 기사를

  * 향후 BIGKINDS/엑셀 데이터와 연결해
  * “실시간 기사 + 프레이밍 히스토리”를 함께 보여주는 기능 검토 중

### 3. 분석 대상 및 시각화 고도화

* 노란봉투법 외에

  * 노동, 부동산, 환경, 젠더, 외교 등 다양한 이슈로 확장
* 타임라인 기반 시각화

  * 이슈별로 **시간에 따른 키워드 변화**를 그래프로 제공
* 사용자 인터랙션

  * 사용자가 직접 기사에 “프레임 태그”를 다는 **크라우드 태깅** 기능 아이디어 검토

---

## 🤝 Contributing & 문의

* 이 프로젝트는 **남민지**와 **김선표(Roflaff)**가 함께 개발하고 있습니다.
* 버그 리포트, 기능 제안, 연구 협업 등은 GitHub **Issues**로 남겨주세요.
* 언론 분석/데이터 저널리즘/뉴스 시각화에 관심 있는 분들의 PR, 언제든 환영합니다! 🙌

```
```
