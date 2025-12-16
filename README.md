# 기계학습과 응용 final project

한국어/영어권 관객의 **영화 리뷰 표현 차이**를 비교하기 위한 프로젝트입니다.  
동일 영화 **기생충(Parasite)**에 대해 **네이버(Naver) 리뷰(한국어)**와 **IMDb 리뷰(영어)**를 수집·전처리하고, 결과를 **R Shiny 대시보드**로 탐색할 수 있게 구성했습니다.

## 분석 관심사(가설/배경)
- 한국어 리뷰는 완곡·간접적 비판(체면/공손성 규범) 경향
- 영어 리뷰는 더 직접적·감정적으로 강한 평가 표현 경향
- 평점 분포도 영어권은 극단값(아주 높음/아주 낮음)이 더 많고, 한국어권은 중간값에 더 몰릴 가능성
---
## 디렉터리 개요
- `shiny-app/` : 평점·단어 분석 Shiny 앱 (주로 `app.R`)
- `r-analysis/` : 워드클라우드/평점 비교 등 R 분석 스크립트 모음
- `data_pipeline/` : 크롤링/전처리/임베딩 생성 등 파이프라인 코드 (Shiny 실행에는 필수 아님)
- `output/` : 미리 수집·전처리된 결과물 (CSV, 임베딩 산출물 등)
---
## 데이터 수집(Data Collection)

### Naver (Korean)
- 네이버 영화 페이지에서 **“관람평” 탭**을 통해 리뷰를 수집합니다.
- 주요 추출 필드(리뷰 카드 단위)
  - 작성자ID, 작성일자, 리뷰 텍스트, 별점, 공감수/비공감수
- 저장 예시
  - `Parasite_naver_reviews.csv`
  - 컬럼 예: `author_id, rating, content, date, like, dislike`

### IMDb (English)
- IMDb 리뷰 페이지에서 **“See all”** → 스크롤 로딩 후 리뷰 카드를 수집합니다.
- 주요 추출 필드
  - author, title, rating(있으면), text
- 저장 예시
  - `Parasite_imdb_reviews.csv`
  - 컬럼 예: `author, title, rating, text`
  ---
  ## 전처리(Data Preprocessing)

### Korean (Naver)
- 토큰화 → 한글 토큰(길이 ≥ 2) 중심 필터링
- 커스텀 불용어(stopwords) 제거
- 리뷰 단위로 다시 결합한 클린 텍스트 생성

### English (IMDb)
- 소문자화 → 토큰화
- 패키지 내장 기본 불용어 + 영화 관련 커스텀 불용어(예: movie, director, spoiler 등) 제거
- 리뷰 단위로 다시 결합한 클린 텍스트 생성
---

