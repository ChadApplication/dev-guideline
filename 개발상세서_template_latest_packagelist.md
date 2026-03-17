# PackageList 개발상세서

> 작성일: 2026-03-17 | 복사원본: `_template_latest`
> 포트: Backend 8020 / Frontend 3020
> 상태: v1.0 구현 완료

---

## 1. 프로젝트

**한 줄 요약**: 현재 컴퓨터에 설치되어 있는 Homebrew, Python pip, uv, CLI 도구 등의 패키지를 모니터링하고 관리하는 로컬 대시보드

**문제 → 해결**: 현재 컴퓨터에 어떤 툴이 설치되어있고 어떻게 사용해야 하는지, 업데이트가 필요한지 파악이 어려움. 패키지들을 소스별/카테고리별로 분류하여 용도 파악, 업데이트 상태 확인, 메모 기록이 가능한 상시 모니터링 도구

---

## 2. 템플릿에서 변경한 것

### run.sh

```
PROJECT_NAME="PackageList"
DEFAULT_BACKEND_PORT=8020
DEFAULT_FRONTEND_PORT=3020
```

### 인증 시스템

- 템플릿의 NextAuth 인증 **제거** (로컬 전용 도구이므로 불필요)
- layout.tsx에서 ThemeProvider, NextAuthProvider, ActivityTracker 제거
- next.config.ts에서 auth 관련 rewrite 제외, 전체 `/api/*` 프록시로 단순화

### backend/main.py

- 템플릿의 세션/인증/설정 관련 API **전체 교체**
- 패키지 스캔/조회/메모/업데이트 전용 API로 재작성

### backend/requirements.txt 추가 패키지

```
(기본만 사용: fastapi, uvicorn, python-multipart, pydantic, python-dotenv)
추가 패키지 없음
```

### frontend 추가 패키지

```
추가 패키지 없음 (기본 Next.js + Tailwind + Lucide React)
```

---

## 3. 역할

인증 없음. 단일 사용자 로컬 도구.

---

## 4. 데이터 모델

### Prisma/DB 사용 안 함

패키지 목록은 JSON 파일로 관리:

```
backend/data/
  packages.json     # 스캔된 전체 패키지 목록 + 사용자 메모
```

### packages.json 스키마

```json
[
  {
    "name": "llm",
    "version": "0.28",
    "description": "Access large language models from the command-line",
    "source": "brew-formula",
    "category": "AI / LLM",
    "memo": "사용자가 입력한 메모",
    "update_available": false,
    "latest_version": null
  }
]
```

### 소스 (source) 값

| source | 설명 | 스캔 방법 |
|--------|------|-----------|
| `brew-formula` | Homebrew formulae | `brew leaves` + `brew info` |
| `brew-cask` | Homebrew casks | `brew list --cask` + `brew info --cask` |
| `pip` | Python pip (pyenv global) | `pip list --format=json` |
| `uv-tool` | uv로 설치된 도구 | `uv tool list` |

### 카테고리 자동 분류 (brew)

brew 패키지는 설명(description) 키워드 기반으로 자동 분류:

| 카테고리 | 키워드 예시 |
|----------|------------|
| AI / LLM | llm, whisper, gemini, ai, model |
| Development | compiler, build, cmake, git, sdk, java, python, rust |
| Search / Text | search, grep, find, fuzzy, json |
| Terminal / System | terminal, shell, tmux, monitor, file manager |
| Media / Image | media, image, video, audio, music, gif |
| Document / Presentation | markdown, document, pdf, slide, typeset |
| GUI App | (모든 cask) |
| Python (pip) | (모든 pip 패키지) |
| Python (uv tool) | (모든 uv 도구) |
| Utility | (기타) |

---

## 5. API

템플릿 API 전체 교체. 아래가 현재 전체 API:

| Method | Path | 설명 |
|--------|------|------|
| GET | /api/health | 서버 상태 확인 |
| POST | /api/packages/scan | 전체 패키지 스캔 (brew, pip, uv 병렬) |
| GET | /api/packages | 패키지 목록 조회 (?category=, ?q= 필터) |
| GET | /api/packages/categories | 카테고리별 패키지 수 조회 |
| PUT | /api/packages/{source}/{name}/memo | 패키지별 메모 저장 |
| POST | /api/packages/check-updates | 업데이트 가능 패키지 확인 (brew outdated, pip outdated) |

---

## 6. 화면

단일 페이지 (`/`). 로그인/회원가입/대시보드 분리 없음.

### 레이아웃

```
┌──────────────────────────────────────────────────┐
│ Header: 🎁 PackageList  158 packages  [Scan] [Check Updates]  │
├──────────┬───────────────────────────────────────┤
│ Sidebar  │  메인 콘텐츠 (접히는 그룹별 테이블)    │
│          │                                       │
│ [검색]   │  ▶ AI / LLM (Homebrew)          9     │
│          │  ▼ Development (Homebrew)       15     │
│ All  158 │    Name    | Ver  | Desc  | Memo      │
│ AI    9  │    cmake   | 3.31 | Cross | ...       │
│ Dev  15  │    git     | 2.49 | Dist  | ...       │
│ Term 13  │                                       │
│ Media 8  │  ▶ Python (pip)                62     │
│ Doc   2  │  ▶ GUI App (Cask)              11     │
│ GUI  11  │                                       │
│ pip  62  │                                       │
│ uv    2  │                                       │
│ Util 31  │                                       │
└──────────┴───────────────────────────────────────┘
```

### UI 요소

- **Header**: 앱 이름, 총 패키지 수, 업데이트 가능 수, Scan/Check Updates 버튼
- **Sidebar**: 검색창 + 카테고리 필터 (클릭 시 메인 필터링)
- **Main**: 소스+카테고리별 접히는 그룹, 각 그룹 안에 테이블 (Name, Version, Description, Memo)
- **Memo 편집**: 클릭 시 인라인 편집, Enter로 저장, Esc로 취소
- **업데이트 표시**: 업데이트 가능 시 버전 옆에 주황색 ↑{latest_version} 표시

---

## 7. 기능

| # | 기능 | 설명 | 우선순위 | 상태 |
|---|------|------|----------|------|
| F01 | 패키지 스캔 | brew/pip/uv 병렬 스캔, JSON 저장 | P0 | ✅ 완료 |
| F02 | 카테고리 필터 | 사이드바에서 카테고리별 필터링 | P0 | ✅ 완료 |
| F03 | 검색 | 이름/설명/메모에서 실시간 검색 | P0 | ✅ 완료 |
| F04 | 메모 저장 | 패키지별 용도 메모, 스캔 후에도 보존 | P0 | ✅ 완료 |
| F05 | 업데이트 확인 | brew outdated + pip outdated 확인 | P1 | ✅ 완료 |
| F06 | 다크 모드 | Tailwind dark: 클래스 지원 | P2 | 부분 |
| F07 | 패키지 삭제/설치 | 대시보드에서 brew uninstall 등 | P2 | 미구현 |

---

## 8. 구현 순서

```
Phase 0: cp -r _template_latest packagelist → run.sh 수정 → venv + npm install     ✅
Phase 1: backend/services/scanner.py (brew, pip, uv 스캔)                           ✅
         backend/services/store.py (JSON 저장/메모 보존)                             ✅
         backend/main.py (5개 API 엔드포인트)                                       ✅
Phase 2: frontend/src/app/page.tsx (단일 대시보드)                                   ✅
         frontend/src/app/layout.tsx (인증 제거, 경량화)                              ✅
         frontend/next.config.ts (API 프록시 단순화)                                 ✅
Phase 3: (향후) 다크 모드 완성, 패키지 삭제/설치 기능                                  미착수
```

---

## 9. 파일 구조

```
packagelist/
├── run.sh                              # 서버 제어 (8020/3020)
├── backend/
│   ├── main.py                         # FastAPI 앱 (5개 엔드포인트)
│   ├── requirements.txt                # fastapi, uvicorn, pydantic, python-dotenv
│   ├── services/
│   │   ├── scanner.py                  # 패키지 스캔 (brew, pip, uv 병렬)
│   │   └── store.py                    # JSON 파일 저장/로드, 메모 보존
│   ├── data/
│   │   └── packages.json               # 스캔 결과 (자동 생성)
│   └── venv/                           # Python 3.12.8 가상환경
└── frontend/
    ├── src/app/
    │   ├── page.tsx                     # 단일 대시보드 (사이드바+테이블)
    │   ├── layout.tsx                   # 경량 레이아웃 (인증 없음)
    │   └── globals.css                  # Tailwind 스타일
    ├── next.config.ts                   # API 프록시 → 8020
    ├── .env.local                       # NEXT_PUBLIC_BACKEND_PORT=8020
    ├── package.json                     # Next.js 15 + React 19 + Tailwind
    └── node_modules/
```

---

## 메모

- 최초 스캔에 약 30~60초 소요 (brew info를 패키지별로 호출하므로)
- 스캔 결과는 `backend/data/packages.json`에 캐시, 재스캔 시 메모는 보존
- 인증/DB/Prisma 미사용 → 템플릿 대비 대폭 경량화
