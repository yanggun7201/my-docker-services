# My Docker Services

여러 프로젝트를 하나의 `docker-compose.yaml`로 관리하는 저장소.

## 구조

```
my_docker_services/
  docker-compose.yaml              # 모든 서비스 정의
  nzkoreapost-scraper/
    Dockerfile                     # NZKoreaPostInterestScraper용
```

각 서비스의 Dockerfile은 이 저장소에 보관하고, 소스 코드는 원본 프로젝트 경로를 build context로 참조한다.

## 등록된 서비스

| 서비스 | 소스 경로 | 설명 |
|--------|----------|------|
| nzkoreapost-scraper | `/Users/yanggun7201/Development/NZKoreaPostInterestScraper` | NZ 한인포스트 게시글 스크래핑 + 환율 알림 (FCM) |
| naver-stock-tracker | `/Users/yanggun7201/Development/NaverStockTracker` | 네이버 주식 급등락/거래량 모니터링 + Slack 알림 |
| naver-stock-consensus-tracker | `/Users/yanggun7201/Development/NaverStockConsensusTracker` | 네이버 주식 컨센서스 추적 + Slack 알림 (puppeteer) |

## 사용법

### 실행 (빌드 + 시작)

```bash
cd /Users/yanggun7201/clawd/general_apps/my_docker_services
docker compose up -d
```

첫 실행 시 이미지 빌드까지 자동으로 진행된다.

### 소스 수정 후 재빌드

```bash
docker compose up -d --build
```

### 특정 서비스만 재빌드

```bash
docker compose up -d --build nzkoreapost-scraper
```

### 특정 서비스만 재시작

```bash
docker compose restart nzkoreapost-scraper
```

### .env 수정 후 재시작

`restart`는 환경변수를 다시 읽지 않는다. `.env` 변경 후에는 `up`을 사용:

```bash
docker compose up -d nzkoreapost-scraper
```

나머지 서비스는 영향 없이 해당 서비스만 환경변수를 다시 읽고 재시작된다.

### 중지

```bash
docker compose down
```

### 로그 확인

```bash
# 전체 로그
docker compose logs -f

# 특정 서비스 로그
docker compose logs -f nzkoreapost-scraper
```

### 상태 확인

```bash
docker compose ps
```

## 새 서비스 추가 방법

1. 서비스 이름으로 디렉토리 생성 후 `Dockerfile` 작성
2. `docker-compose.yaml`의 `services:`에 추가
3. `docker compose up -d`로 실행

## 참고

- native 모듈(sqlite3 등)이 있는 Node.js 프로젝트는 Dockerfile이 필요하다 (macOS용 바이너리가 Linux 컨테이너에서 작동하지 않음)
- 순수 JS 패키지만 사용하는 프로젝트는 Dockerfile 없이 volume mount로도 가능하다
- 환경변수(`.env`)는 원본 프로젝트의 파일을 직접 참조한다
- SQLite 데이터는 named volume에 저장되어 컨테이너를 재시작해도 유지된다
