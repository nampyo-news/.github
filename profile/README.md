# 남표뉴스 (Nampyo News)

> **빅카인즈(BIGKINDS) 기반 진보/보수 언론 보도 경향 분석 & 시각화 웹사이트**  
> 개발: 김선표(Roflaff) · 남민지(깃허브 하이디)  
> 역할: 남민지 – 기획 · 프론트엔드 / 김선표 – 백엔드

---

## 1. 프로젝트 소개

**남표뉴스**는 한국 주요 일간지의 뉴스 데이터를 수집·분석해  
**진보/보수 진영별 보도 프레임과 키워드 사용 패턴을 시각적으로 보여주는** 프로젝트입니다.

- 데이터 소스
  - **BIGKINDS API**: 기사 본문/메타데이터 및 키워드(TMS) 수집
  - **Naver Search API**: 실시간/최근 뉴스 목록 스트리밍
- 분석 단위
  - 언론사별 기사
  - 진영별(blue: 진보, red: 보수) 키워드 빈도
- 제공 기능(요약)
  - 검색어·기간·언론사 선택에 따른 **뉴스 목록 조회**
  - **진영별 상위 키워드 TOP 10** 자동 집계
  - 프론트엔드에서 **워드 크기·맵 형태 등으로 시각화**(예: 폰트 크기, 키워드 관계 맵)

---

## 2. 연구/분석 배경: “노란봉투법” 사례

본 프로젝트의 초기 버전은 **노란봉투법** 관련 보도에 대한 학술 연구를 기반으로 합니다.

- **분석 기간**: 2025-07-01 ~ 2025-10-28 (119일)  
- **대상 언론사 (4개)**  
  - 진보 성향: 경향신문, 한겨레  
  - 보수 성향: 조선일보, 동아일보  
- **총 기사 수**: 744건
- **핵심 분석 방법**
  - BIGKINDS에서 제공하는 **키워드(TMS, 명사 집합)**를 활용
  - 언론사를 **진보(blue) / 보수(red)** 진영으로 분류
  - 진영별로 **키워드 출현 빈도(%)**를 계산해 상위 10개를 비교

### 2.1 진보 vs 보수: 상위 키워드 예시

| 구분 | 상위 키워드(일부) | 특징 |
|------|-------------------|------|
| **진보 언론 (경향·한겨레)** | 봉투법(1.30%), 대통령(0.89%), 국민(0.80%), 정부(0.64%), 기업(0.55%), **원청(0.53%)**, **노동자(0.46%)**, **교섭(0.44%)**, 노조(0.45%) 등 | 진보 진영 고유 키워드: **‘노동자’, ‘원청’, ‘교섭’** → 노동권, 집단교섭, 구조적 관계를 강조하는 프레임 |
| **보수 언론 (조선·동아)** | 봉투법(1.46%), 대통령(1.28%), 기업(0.97%), 국민(0.76%), 정부(0.72%), **민주당(0.69%)**, 통과(0.48%), **대표(0.46%)**, 노조(0.45%), **한국(0.39%)** 등 | 보수 진영 고유 키워드: **‘민주당’, ‘대표’, ‘한국’** → 정치 주체·정당·국가 프레임, 그리고 **기업(0.97%)** 비율이 진보보다 1.76배 높게 등장 |

### 2.2 보도 프레임 차이

- **보수 언론(조선/동아)**  
  - 예: “노란봉투법 헌법소원 각하… 청구 기업에 노조 없어” 등  
  - **법안의 절차적 정당성, 실효성, 기업 부담**에 초점을 둔 제목과 키워드 사용  
  - 키워드 태그: ‘봉개’, ‘기업’, ‘노조’, ‘청구’, ‘부적격’ 등
- **진보 언론(경향/한겨레)**  
  - 노란봉투법을 **노동권 보호, 원청 책임, 집단적 협의 과정** 중심의 프레임으로 접근  
  - H1-1(“진보 언론은 노동 주체의 권리와 집단적 가치를 강조하는 키워드를 더 많이 사용할 것이다”)를 지지

**남표뉴스**는 이와 같은 **진영별 키워드·프레임 차이**를 웹에서 직관적으로 확인할 수 있게 만드는 것이 목표입니다.

---

## 3. 기능 개요

### 3.1 사용자 기능

- 🔍 **검색어 기반 뉴스 조회**
  - 검색어, 기간, 언론사를 설정해 관련 기사 목록과 메타 데이터를 조회
- 📈 **진영별 키워드 상위 10개 비교**
  - 진보(blue) / 보수(red) 언론별 상위 키워드 및 비율을 표/그래프로 확인
- 📰 **실시간 뉴스 스트리밍 (Naver API + SSE)**
  - 15초 간격으로 최신 검색 결과를 스트림으로 받아 프론트에서 실시간 UI 업데이트
- 🧠 **프레이밍/담론 구조 파악**
  - 동일 이슈에 대해 각 진영이 어떤 단어를 중심으로 이야기를 구성하는지 직관적으로 확인

### 3.2 기술적 특징

- FastAPI 기반 비동기 백엔드
- httpx AsyncClient로 Naver/BIGKINDS API 비동기 호출
- **Server-Sent Events(SSE)** 기반 실시간 스트림
- pydantic v2 & pydantic-settings를 활용한 설정/스키마 관리
- 시작 시 외부 API 상태 점검 파이프라인
- KST 타임존 기반 **파일 로깅(레벨별 분리)**

---

## 4. 아키텍처 개요

### 4.1 전반 구조

- **프론트엔드**
  - React 기반 SPA  
  - 피그마로 UI 설계 후, [v0.app](https://v0.app/)을 활용해 초기 구조 생성  
  - VS Code + Copilot “바이브 코딩”으로 컴포넌트/스타일 개선  
  - 별도 레포: [`nampyo-news/nampyo-news-FE`](https://github.com/nampyo-news/nampyo-news-FE)
- **백엔드 (이 레포)**
  - FastAPI + Uvicorn
  - Naver Search API & BIGKINDS API 연동
  - 키워드 집계 로직 및 진영 분류 로직 포함

### 4.2 백엔드 구성 파일

- `nampyo_news/main.py`
  - FastAPI 앱 초기화, CORS, 라우터 마운트(`/public`)
  - 루트 엔드포인트(`/`) 제공 (헬스 체크 겸용)
- `nampyo_news/router.py`
  - API 버전 네임스페이스: `/public/v1`
  - 뉴스 도메인 라우터: `/public/v1/news/*`
- `nampyo_news/apis/v1/news.py`
  - `GET /sse` : Naver 뉴스 검색 결과 SSE 스트림
  - `POST /bigkinds/get_info` : BIGKINDS 호출 후 분석 결과 반환
- `nampyo_news/services/v1/naver_api.py`
  - Naver Search API 호출 및 SSE 제너레이터
- `nampyo_news/services/v1/bigkinds_api.py`
  - BIGKINDS 호출, 응답 정규화, 진영별 키워드 집계
- `nampyo_news/schemas/request.py`
  - BIGKINDS 요청용 pydantic 모델 (중첩 구조 포함)
- `nampyo_news/schemas/response.py`
  - 표준 성공/에러 응답, `NewsInfoData` 등 데이터 모델
- `nampyo_news/start_server.py`
  - 서버 시작 시 **Naver/BIGKINDS API 상태 점검** 파이프라인
- `nampyo_news/config.py`
  - `.env.local` 기반 설정 로딩, API 키/언론사 분류(blue/red) 등
- `nampyo_news/logger.py`
  - INFO / WARNING / ERROR를 파일로 분리 기록 + 콘솔 출력
  - KST 기반 시각 포맷 일원화

---

## 5. 데이터 흐름

### 5.1 실시간 뉴스 스트림 (Naver API + SSE)

1. 클라이언트 → `GET /public/v1/news/sse?query={검색어}`
2. 백엔드 → Naver Search API 비동기 호출
3. 응답 JSON을 `data: {...}\n\n` 형태의 SSE 프레임으로 15초 주기 전송
4. 클라이언트 → 스트림을 받아 리스트/카드 형태 등으로 표현

> 15초 간격으로 백엔드가 검색어에 기반한 최신 뉴스를 끊임없이 푸시합니다.

### 5.2 BIGKINDS 기반 키워드 분석

1. 클라이언트 → `POST /public/v1/news/bigkinds/get_info`
   - `query`, `from_timestamp`, `to_timestamp`, `provider(언론사 목록)` 등 전달  
2. 백엔드 → BIGKINDS API POST 호출
3. 응답의 `return_object.documents`를 파싱
4. `tms_raw_stream`에서 키워드(명사) 리스트를 추출
5. 언론사 → 진영(blue/red)으로 매핑
6. 진영별로 키워드 출현 빈도 계산 후 **상위 10개** 집계
7. 기사 리스트 + `top_n_keywords`를 함께 반환

> “사용자 입력(검색어 + 기간) → BIGKINDS 호출 → 키워드 빈도 계산 → 진영별 TOP 10 키워드 반환”  
> 이 과정을 한 번의 API 요청으로 처리합니다.

---

## 6. API 설계

### 6.1 실시간 뉴스 스트림 (SSE)

- **Endpoint**
  - `GET /public/v1/news/sse`
- **Query**
  - `query` (string, required) — 검색어
- **Response**
  - `Content-Type: text/event-stream`
  - 각 이벤트:  
    ```text
    data: {네이버 뉴스 검색 결과 JSON}
    
    ```
- **핵심 구현**
  - `services/v1/naver_api.py::news_event_generator()`
  - 15초 주기로 `fetch_naver_news()` 호출
  - 예외/연결 종료 시 루프 종료

### 6.2 BIGKINDS 뉴스 정보 조회 & 키워드 분석

- **Endpoint**
  - `POST /public/v1/news/bigkinds/get_info`
- **Request Body** (`schemas/request.py::APIRequest`)
  - `query: str | None`
  - `from_timestamp: str | None` (YYYY-MM-DD)
  - `to_timestamp: str | None` (YYYY-MM-DD)
  - `provider: List[str] | None`  
    - 미지정 시 Config의 blue/red 언론사 전체 사용
  - `return_size: int | None = 1000` (내부적으로 최대 10,000까지 처리 로직 포함)
- **Response** (`schemas/response.py`)
  - `APISuccessResponse`:
    - `status: "success"`
    - `data: List[NewsInfoData]`
    - `top_n_keywords: { "blue": [(keyword, count), ...], "red": [...] }`
  - `APIErrorResponse`:
    - `status: "error"`
    - `data: 에러 상세 정보`
- **내부 동작**
  - 날짜 미지정 시 기본값: “최근 24시간”
  - BIGKINDS 포맷에 맞게 Argument/PublishedAt 스키마 직렬화
  - 언론사 → 진영 매핑 후 `collections.Counter`로 키워드 상위 10개 계산

---

## 7. 외부 서비스 연동

### 7.1 Naver Search API

- **Endpoint**
  - `GET https://openapi.naver.com/v1/search/news.json`
- **Auth**
  - `X-Naver-Client-Id: Config().x_naver_client_id`
  - `X-Naver-Client-Secret: Config().x_naver_client_secret`
- **Params**
  - `query`, `display` (기본 10) 등
- **구현**
  - `fetch_naver_news()` — httpx.AsyncClient로 비동기 호출

### 7.2 BIGKINDS API

- **Endpoint**
  - `POST https://tools.kinds.or.kr/search/news`
- **Auth**
  - Request Body의 `access_key = Config().bigkinds_api_key`
- **Payload**
  - `NewsRequest(Argument(...))` → `model_dump(by_alias=True)`로 직렬화
- **후처리**
  - `return_object.documents`에서 주요 필드 추출
  - `tms_raw_stream` 문자열을 줄바꿈 기준으로 잘라 키워드 리스트 생성
  - 언론사별 진영 매핑 후 키워드 빈도 집계

---

## 8. 설치 및 실행

### 8.1 요구 사항

- Python **3.10**
- [Poetry](https://python-poetry.org/)
- BIGKINDS / Naver API 키

### 8.2 환경 변수 설정 (`.env.local` 예시)

```env
X_NAVER_CLIENT_ID=your_naver_client_id
X_NAVER_CLIENT_SECRET=your_naver_client_secret
BIGKINDS_API_KEY=your_bigkinds_api_key

# 선택: 언론사 진영 분류 (JSON 문자열 형태 추천)
BLUE_PROVIDERS=["경향신문", "한겨레"]
RED_PROVIDERS=["조선일보", "동아일보"]
