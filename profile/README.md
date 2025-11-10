📰 남표뉴스 (Nampyo News)

빅카인즈 기반 실시간 뉴스 분석 및 언론 보도 경향 시각화 웹사이트

'남표뉴스'는 특정 사회적 이슈에 대한 주요 언론사들의 보도 경향을 빅카인즈(BIGKINDS) 데이터를 기반으로 분석하고, 그 결과를 시각적으로 제공하여 사용자가 미디어 지형을 객관적으로 이해할 수 있도록 돕는 웹 서비스입니다.

🧑‍💻 개발팀 및 역할

이름 (GitHub)

역할

담당 기술 스택

김선표 (Roflaff)

백엔드 개발 및 시스템 아키텍처

Python (FastAPI), BIGKINDS API, Naver Search API, SSE

남민지 (하이디)

기획 및 프론트엔드 개발

기획, UI/UX 설계 (Figma), 프론트엔드 구현

✨ 주요 기능

실시간 뉴스 스트리밍: Naver Search API를 통해 검색어 기반의 최신 뉴스 목록을 SSE (Server-Sent Events) 방식으로 실시간 제공합니다.

진영별 키워드 분석: BIGKINDS API를 활용하여 수집된 뉴스 기사의 메타 정보 및 키워드를 추출하고, 언론사 진영(진보/보수)별 키워드 빈도 상위 통계를 제공합니다.

보도 경향 시각화: 분석된 진영별 키워드 데이터를 프론트엔드에서 직관적인 시각화 자료로 구현하여 언론사 간의 담론 구성 차이를 명확하게 보여줍니다.

🔬 핵심 분석 방법론: '노란봉투법' 사례 연구

본 프로젝트의 핵심 분석 방법론은 진영별 키워드 빈도 분석 및 비교입니다.

1. 분석 개요

분석 대상 이슈: 노란봉투법 (노동조합 및 노동관계조정법 개정안)

분석 기간: 2025년 7월 1일 ~ 10월 28일 (119일간)

분석 언론사: 경향신문(진보), 한겨레(진보), 조선일보(보수), 동아일보(보수) (총 744건의 기사 수집)

분석 목적: 동일한 이슈에 대해 진보 및 보수 언론이 어떤 개념과 용어를 중심으로 담론을 구성하는지 비교 분석합니다.

2. 분석 결과 요약

동일 이슈에 대해 진보 언론과 보수 언론은 뚜렷하게 구별되는 키워드 사용 패턴을 보였습니다.

진영

고유 키워드 (상위 10개 중)

강조 포인트

진보 (경향신문, 한겨레)

노동자, 원청, 교섭

노동 주체의 권리와 집단적 협의 과정 (H1-1 지지)

보수 (조선일보, 동아일보)

민주당, 한국, 대표

법안의 절차적 문제, '기업'의 부담 및 시장 효율성 (H1-2 부분 지지)

특히, '기업' 키워드의 등장 비율은 보수 언론이 0.97%로 진보 언론(0.55%)에 비해 약 1.76배 높게 나타나, 경제 주체의 부담을 부각하는 경향을 확인할 수 있었습니다.

⚙️ 백엔드 아키텍처 (FastAPI)

본 프로젝트의 백엔드는 고성능 비동기 처리를 위해 FastAPI를 기반으로 구축되었습니다.

시스템 개요

프레임워크: FastAPI (Python)

특징: 비동기 서버, httpx.AsyncClient를 사용한 외부 API 비동기 호출, Server-Sent Events (SSE) 지원.

설정 관리: pydantic-settings를 통한 환경 변수(X_NAVER_CLIENT_ID, BIGKINDS_API_KEY 등) 로드.

로깅: 파일 기반 다중 레벨 로깅(INFO/WARNING/ERROR) 및 KST(한국 표준시) 타임존 적용.

부트스트랩: 서버 시작 시 외부 API 상태 점검 파이프라인(start_server.py) 실행.

아키텍처 구성 요소

계층/모듈

역할

주요 기능

router.py

라우팅 계층

API 버전 네임스페이스 (/public/v1) 관리

news.py

도메인 엔드포인트

Naver SSE 스트림 제공, BIGKINDS 데이터 분석 처리

naver_api.py

서비스 계층

Naver API 호출 및 SSE 제너레이터 구현

bigkinds_api.py

서비스 계층

BIGKINDS 호출, 결과 정규화 및 진영별 키워드 집계

request.py / response.py

스키마 계층

Pydantic을 활용한 요청/응답 스키마 정의

데이터 흐름 개요

실시간 스트리밍: 클라이언트 → GET /sse → Naver API (비동기) → 15초 주기 SSE 스트림 전송.

키워드 분석: 클라이언트 → POST /bigkinds/get_info → BIGKINDS API 호출 → 문서 목록 파싱/필터링 → 언론사 진영 매핑 → 진영별 키워드 빈도 상위 10개 집계 → 최종 응답.

🔌 API 엔드포인트

#

Endpoint

Method

설명

1

/public/v1/news/sse

GET

Naver 뉴스 검색 결과를 15초 주기로 SSE 스트리밍 제공. (Query: query 필수)

2

/public/v1/news/bigkinds/get_info

POST

BIGKINDS에서 뉴스 목록 수집, 가공 후 키워드 상위 통계 반환. (Body: query, from_timestamp, to_timestamp, provider 등)

🛠️ 개발 및 협업 환경

본 프로젝트는 효율적인 협업 및 개발 속도 향상을 위해 AI 기반 도구들을 적극적으로 활용했습니다.

Figma 디자인: 서비스의 UI/UX 설계 및 시각화 구상은 Figma를 통해 진행되었습니다.

v0 기반 구조 생성: AI 기반 코드 생성 도구인 v0를 활용하여 프로젝트의 기본 프론트엔드 구조를 신속하게 생성하고 nampyo-news-FE 레포지토리에 옮겨 개발을 시작했습니다.

Copilot/Vibe Coding: 백엔드 개발 과정에서 VSCode 내장 기능을 활용하여 실시간 코드 제안 및 디버깅, API 연동 로직 구현을 효율적으로 진행했습니다. 특히 Simple Browser의 'Add element to chat' 기능을 통해 프론트엔드 요소 기반으로 백엔드 요구사항을 자연어로 전달하여 협업하였습니다.

🚀 로컬 환경에서 시작하기

1. 런타임 및 의존성

Python: 3.10 이상

의존성 관리: Poetry

주요 패키지: FastAPI, httpx, pydantic v2, sse-starlette

2. 환경 변수 설정

프로젝트 루트에 .env.local 파일을 생성하고 다음 키들을 설정해야 합니다.

# .env.local 예시
X_NAVER_CLIENT_ID="YOUR_NAVER_CLIENT_ID"
X_NAVER_CLIENT_SECRET="YOUR_NAVER_CLIENT_SECRET"
BIGKINDS_API_KEY="YOUR_BIGKINDS_API_KEY"

# (선택 사항) 언론사 진영 설정 오버라이드 (기본값 존재)
# BLUE_PROVIDERS='["경향신문", "한겨레"]'
# RED_PROVIDERS='["조선일보", "동아일보"]'


3. 서버 실행

의존성 설치:

poetry install


개발 서버 실행 (자동 리로드 포함):

poetry run uvicorn nampyo_news.main:app --host 0.0.0.0 --port 8000 --reload


API 문서 확인:
서버 시작 후 http://localhost:8000/docs에서 OpenAPI 문서를 확인할 수 있습니다.
