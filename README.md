
## 📁 프로젝트 구조

```
jira-issue-finder/
│
├── src/
│   ├── __init__.py
│   ├── jira_crawler.py         # JIRA 이슈 크롤링
│   ├── text_preprocessor.py    # 텍스트 전처리 (불용어, 유사어 처리)
│   ├── issue_matcher.py        # 이슈 유사도 매칭
│   ├── issue_storage.py        # 이슈 데이터 저장/로드
│   └── cli_handler.py          # CLI 입출력 처리
│
├── data/
│   ├── raw_issues.json         # 크롤링 원본 데이터
│   └── processed_issues.json   # 전처리 완료 데이터
│
├── config/
│   ├── __init__.py
│   ├── settings.py             # 애플리케이션 설정
│   ├── browser_config.json     # 브라우저 인증 정보
│   └── stopwords.txt           # 불용어 사전
│
├── logs/
│   └── batch_YYYYMMDD.log      # 배치 실행 로그
│
├── .env                        # 환경 변수
├── .gitignore                  # Git 제외 파일
├── requirements.txt            # Python 패키지 목록
├── README.md                   # 프로젝트 문서
│
├── run_batch.py                # 배치 작업 실행 스크립트
└── run_search.py               # 검색 실행 스크립트
```

## 📄 파일 설명

### 실행 스크립트
- **run_batch.py**: 배치 작업 실행 (크롤링 → 전처리 → 저장)
- **run_search.py**: 검색 실행 (데이터 로드 → 사용자 입력 → 매칭 → 출력)

### config/ - 설정 파일
- **settings.py**: 애플리케이션 전역 설정
- **browser_config.json**: 브라우저 및 인증 정보
- **stopwords.txt**: 텍스트 전처리용 불용어 사전

### data/ - 데이터 저장소
- **raw_issues.json**: 크롤링한 원본 이슈 데이터
- **processed_issues.json**: 전처리가 완료된 이슈 데이터

### logs/ - 로그 파일
- **batch_YYYYMMDD.log**: 배치 작업 실행 로그 (날짜별)

## 🔄 작업 흐름

### 1️⃣ 배치 작업 (데이터 수집 및 전처리)

```bash
python run_batch.py
```

```
run_batch.py 실행
    ↓
jira_crawler.py → JIRA에서 이슈 크롤링
    ↓
issue_storage.py → raw_issues.json에 원본 저장
    ↓
text_preprocessor.py → 텍스트 전처리 (불용어 제거, 유사어 처리)
    ↓
issue_storage.py → processed_issues.json에 저장
    ↓
logs/batch_YYYYMMDD.log → 실행 로그 기록
```

### 2️⃣ 검색 작업 (유사 이슈 찾기)

```bash
python run_search.py
```

```
run_search.py 실행
    ↓
issue_storage.py → processed_issues.json 데이터 로드
    ↓
cli_handler.py → 사용자 입력 받기
    ↓
text_preprocessor.py → 사용자 입력 전처리
    ↓
issue_matcher.py → 유사도 계산 및 매칭
    ↓
cli_handler.py → 결과 출력 (원본 제목 + URL)
```

## 🚀 사용 방법

### 1. 환경 설정
```bash
pip install -r requirements.txt
```

### 2. 설정 파일 작성
- `config/browser_config.json`: JIRA 인증 정보 입력
- `config/stopwords.txt`: 불용어 목록 작성
- `.env`: 환경 변수 설정

### 3. 배치 실행 (이슈 수집)
```bash
python run_batch.py
```

### 4. 검색 실행
```bash
python run_search.py
```