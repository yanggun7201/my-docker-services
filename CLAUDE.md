# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

여러 개인 프로젝트를 하나의 `docker-compose.yaml`로 실행/관리하는 저장소. 소스 코드는 각 프로젝트의 원본 경로를 참조하고, Dockerfile만 이 저장소에 보관한다.

## Commands

```bash
docker compose up -d              # 전체 실행 (첫 실행 시 빌드 포함)
docker compose up -d --build      # 소스 수정 후 재빌드 + 실행
docker compose up -d --build <서비스명>  # 특정 서비스만 재빌드
docker compose down               # 전체 중지
docker compose logs -f <서비스명>  # 로그 확인
docker compose ps                 # 상태 확인
```

## Architecture

- `docker-compose.yaml`: 모든 서비스를 정의하는 단일 파일
- `<서비스명>/Dockerfile`: native 모듈이나 시스템 의존성이 필요한 프로젝트용
- 소스 코드는 `build.context`로 원본 프로젝트 경로를 직접 참조
- `.env` 파일도 원본 프로젝트의 것을 `env_file`로 참조

## Dockerfile이 필요한 경우 vs 불필요한 경우

- **필요**: native C++ 모듈(sqlite3), 시스템 의존성(puppeteer/Chromium) — macOS 바이너리가 Linux 컨테이너에서 작동하지 않음
- **불필요**: 순수 JS 패키지만 사용 — `image: node:22-alpine` + volume mount로 충분

## 서비스 추가 시

1. native 모듈 여부 확인 → Dockerfile 필요 여부 결정
2. `package-lock.json`의 `lockfileVersion`으로 Node 버전 호환성 확인
3. Dockerfile이 필요하면 `<서비스명>/Dockerfile` 생성
4. `docker-compose.yaml`의 `services:`에 추가
5. `README.md` 서비스 테이블 업데이트
