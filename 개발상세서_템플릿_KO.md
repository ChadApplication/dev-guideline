# [프로젝트명] 개발상세서

> 작성일: YYYY-MM-DD
> 버전: 0.1 (초안)
> 기반 템플릿: `_template_latest`
> 이 문서는 Claude Code와의 협업에 최적화된 구조입니다.

---

## 0. 이 문서의 사용법

이 문서는 Claude Code에게 프로젝트를 설명하고 구현을 위임하기 위한 **단일 진실 소스**입니다.

**작성 순서**: 1장부터 순서대로 채워가세요. 각 장이 다음 장의 입력이 됩니다.
**Claude Code 활용**: 완성된 섹션을 Claude Code에 전달하면 해당 부분을 구현합니다.

```
1. 프로젝트 개요  →  무엇을, 왜 만드는가
2. 사용자 정의    →  누가 쓰는가
3. 기능 목록      →  무엇을 할 수 있는가
4. 데이터 모델    →  어떤 데이터가 필요한가
5. API 설계       →  프론트-백 간 계약
6. 화면 설계      →  사용자가 보는 것
7. 구현 단계      →  어떤 순서로 만드는가
```

---

## 1. 프로젝트 개요

### 1.1 한 줄 요약

> [이 앱이 무엇인지 한 문장으로]

### 1.2 배경 및 목적

- **문제**: [현재 어떤 문제가 있는가]
- **해결**: [이 앱이 어떻게 해결하는가]
- **대상**: [누가 사용하는가]

### 1.3 기술 스택

| 레이어 | 기술 | 비고 |
|--------|------|------|
| Backend | FastAPI (Python 3.12) | `_template_latest` 기반 |
| Frontend | Next.js 15+ / React 19 / TypeScript | App Router |
| DB | SQLite (개발) → PostgreSQL (운영) | Prisma ORM |
| 인증 | NextAuth.js v5 | Credentials Provider |
| 스타일 | Tailwind CSS | |
| 아이콘 | Lucide React | |
| AI/LLM | (사용 시 명시) | OpenAI / Anthropic / 로컬 |

### 1.4 범위 정의

**포함**:
- [ ] (이 프로젝트에서 반드시 구현할 것)
- [ ]
- [ ]

**제외** (향후 고려):
- (이번 버전에서 구현하지 않을 것)

---

## 2. 사용자 정의

### 2.1 역할 (Roles)

| 역할 | 설명 | 기본 권한 |
|------|------|-----------|
| ADMIN | 시스템 관리자 | 모든 데이터 접근, 사용자 관리 |
| USER | 일반 사용자 | 자신의 데이터만 접근 |
| (추가 역할) | | |

### 2.2 사용자 시나리오

**시나리오 1**: [시나리오 이름]
```
사용자: [역할]
목표: [달성하려는 것]
과정:
  1. [행동]
  2. [행동]
  3. [행동]
결과: [기대 결과]
```

**시나리오 2**: [시나리오 이름]
```
사용자: [역할]
목표:
과정:
  1.
  2.
결과:
```

---

## 3. 기능 목록

### 3.1 기능 매트릭스

각 기능에 우선순위를 지정합니다. Claude Code는 P0부터 순서대로 구현합니다.

| ID | 기능 | 설명 | 우선순위 | 역할 |
|----|------|------|----------|------|
| F01 | | | P0 (필수) | |
| F02 | | | P0 (필수) | |
| F03 | | | P1 (중요) | |
| F04 | | | P2 (선택) | |

### 3.2 기능 상세

각 기능의 상세 사양입니다. Claude Code가 이 섹션을 읽고 구현합니다.

#### F01: [기능명]

**설명**: [이 기능이 무엇을 하는지]

**입력**:
- [사용자가 제공하는 것]

**처리**:
- [시스템이 수행하는 것]

**출력**:
- [사용자가 받는 결과]

**제약조건**:
- [크기 제한, 형식 제한 등]

**에러 처리**:
- [입력이 없을 때] → [동작]
- [서버 오류 시] → [동작]

---

#### F02: [기능명]

**설명**:

**입력**:
-

**처리**:
-

**출력**:
-

---

## 4. 데이터 모델

### 4.1 엔티티 정의

Prisma 스키마 형식으로 정의합니다. Claude Code가 이를 바로 `schema.prisma`에 반영합니다.

```prisma
// === 사용자 ===
model User {
  id        String   @id @default(cuid())
  username  String?  @unique
  email     String?  @unique
  password  String?
  name      String?
  role      String   @default("USER")
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  // Relations
  // projects  Project[]
}

// === (엔티티 추가) ===
// model Project {
//   id        String   @id @default(cuid())
//   title     String
//   userId    String
//   user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)
//   createdAt DateTime @default(now())
//   updatedAt DateTime @updatedAt
// }
```

### 4.2 파일 시스템 데이터 (해당 시)

백엔드에서 파일로 관리하는 데이터가 있다면 구조를 정의합니다.

```
backend/temp_uploads/
  {user_id}/
    {session_id}/
      research_info.json      # 세션 메타데이터
      processed_data.csv      # 처리된 데이터
      {step}_results.json     # 각 단계 결과
```

### 4.3 엔티티 관계도 (텍스트)

```
User 1──N Project
Project 1──N Session
Session 1──N Result
```

---

## 5. API 설계

### 5.1 엔드포인트 목록

Claude Code가 이 표를 보고 백엔드 라우터를 생성합니다.

| Method | Path | 설명 | 인증 | 역할 |
|--------|------|------|------|------|
| GET | /api/health | 서버 상태 확인 | X | 전체 |
| POST | /api/sessions | 새 세션 생성 | O | USER+ |
| GET | /api/sessions | 세션 목록 | O | 역할별 필터 |
| DELETE | /api/sessions/{id} | 세션 삭제 | O | 소유자/ADMIN |
| | | | | |
| | | | | |

### 5.2 요청/응답 상세

#### POST /api/sessions

**Request**:
```json
{
  "title": "string (required)",
  "description": "string (optional)"
}
```

**Response 201**:
```json
{
  "status": "ok",
  "session_id": "uuid-string"
}
```

**Response 401**:
```json
{
  "detail": "Not authenticated"
}
```

---

#### GET /api/sessions?role={role}

**Query Params**:
- `role`: ADMIN | MANAGER | USER | GUEST

**Response 200**:
```json
{
  "sessions": [
    {
      "id": "uuid",
      "title": "string",
      "owner_id": "string (ADMIN/MANAGER only)",
      "created_at": "ISO datetime",
      "updated_at": "ISO datetime"
    }
  ]
}
```

---

## 6. 화면 설계

### 6.1 화면 목록

| ID | 화면 | 경로 | 설명 | 인증 |
|----|------|------|------|------|
| S01 | 로그인 | /login | 이메일/비밀번호 로그인 | X |
| S02 | 회원가입 | /register | 계정 생성 | X |
| S03 | 대시보드 | /dashboard | 프로젝트 목록, 생성, 관리 | O |
| S04 | (메인 기능) | /workspace/{id} | | O |
| S05 | 설정 | /settings | 사용자 설정, LLM 키 관리 | O |
| S06 | 관리자 | /admin | 사용자 관리 (ADMIN only) | O |

### 6.2 화면 상세

#### S03: 대시보드

**레이아웃**:
```
┌─────────────────────────────────────────────────┐
│ Header: 로고 | 프로젝트명 | [사용자메뉴] [설정]  │
├─────────────────────────────────────────────────┤
│                                                 │
│  [+ 새 프로젝트]                                 │
│                                                 │
│  ┌──────────────┐  ┌──────────────┐             │
│  │ 프로젝트 카드  │  │ 프로젝트 카드  │             │
│  │ 제목          │  │ 제목          │             │
│  │ 설명...       │  │ 설명...       │             │
│  │ 날짜 | 삭제   │  │ 날짜 | 삭제   │             │
│  └──────────────┘  └──────────────┘             │
│                                                 │
└─────────────────────────────────────────────────┘
```

**동작**:
- 프로젝트 카드 클릭 → `/workspace/{id}`로 이동
- [+ 새 프로젝트] → 제목 입력 모달 → 생성 후 workspace로 이동
- 삭제 → 확인 다이얼로그 → 삭제 후 목록 갱신

**역할별 차이**:
- ADMIN: 모든 사용자의 프로젝트 표시 (소유자 라벨 포함)
- MANAGER: ADMIN 소유 제외, 나머지 전체
- USER: 자신의 프로젝트만
- GUEST: 제목/설명만 (읽기 전용)

---

#### S04: [메인 기능 화면]

**레이아웃**:
```
┌─────────────────────────────────────────────────┐
│ Header: [← 목록] | 프로젝트 제목 | [설정]        │
├────────────┬────────────────────────────────────┤
│            │                                    │
│  사이드바   │          메인 콘텐츠               │
│  (단계별    │                                    │
│   네비)    │                                    │
│            │                                    │
│  Step 1 ✓  │                                    │
│  Step 2 ●  │                                    │
│  Step 3    │                                    │
│            │                                    │
├────────────┴────────────────────────────────────┤
│ Footer: [이전] [다음] | 진행상태                  │
└─────────────────────────────────────────────────┘
```

---

## 7. 구현 단계

Claude Code에게 단계별로 지시할 때 이 순서를 따릅니다.

### Phase 0: 프로젝트 셋업

```
지시: _template_latest를 복사하여 {프로젝트명} 생성.
      run.sh의 PROJECT_NAME과 포트 변경.
      backend venv 생성, frontend npm install.
```

**완료 기준**: `./run.sh start`로 양쪽 서버 정상 기동

### Phase 1: 데이터 모델 + 인증

```
지시: 4장의 Prisma 스키마를 frontend/prisma/schema.prisma에 반영.
      npx prisma db push로 DB 생성.
      기존 인증 시스템 활용 (auth.ts, login, register 페이지).
```

**완료 기준**: 회원가입 → 로그인 → 대시보드 접근 가능

### Phase 2: 핵심 API

```
지시: 5장의 API 엔드포인트를 backend/main.py에 구현.
      services/ 디렉토리에 비즈니스 로직 분리.
      각 엔드포인트 curl로 테스트 가능.
```

**완료 기준**: 모든 API가 정상 응답 (curl 테스트)

### Phase 3: 대시보드 UI

```
지시: 6장 S03의 대시보드 화면 구현.
      기존 _template의 대시보드를 기반으로 수정.
      역할별 필터링 적용.
```

**완료 기준**: 대시보드에서 프로젝트 CRUD 동작

### Phase 4: 메인 기능 UI

```
지시: 6장 S04의 메인 기능 화면 구현.
      [구체적인 기능별 지시 추가]
```

**완료 기준**: [기능별 완료 기준]

### Phase 5: 마무리

```
지시: 에러 처리, 로딩 상태, 빈 상태 UI 추가.
      다크 모드 확인.
      반응형 레이아웃 확인.
```

**완료 기준**: 전체 플로우 시나리오 통과

---

## 8. 설정 및 환경변수

### backend/.env

```env
# 서버
PORT=8010
HOST=0.0.0.0

# CORS
ALLOWED_ORIGINS=http://localhost:3010

# AI/LLM (해당 시)
# OPENAI_API_KEY=
# ANTHROPIC_API_KEY=

# (프로젝트별 추가)
```

### frontend/.env.local

```env
NEXT_PUBLIC_BACKEND_PORT=8010

# NextAuth
AUTH_SECRET=your-secret-key-here

# (프로젝트별 추가)
```

---

## 9. Claude Code 협업 팁

### 지시 패턴

**기능 구현 요청**:
```
개발상세서 F01 기능을 구현하라.
- backend: services/{service_name}.py에 로직 작성
- backend: main.py에 API 엔드포인트 추가
- frontend: src/app/workspace/page.tsx에 UI 구현
```

**버그 수정 요청**:
```
{화면}에서 {증상} 발생.
기대 동작: {올바른 동작}
현재 동작: {잘못된 동작}
```

**구조 변경 요청**:
```
개발상세서 4장 데이터 모델을 업데이트했다.
{변경 내용} 반영하라.
```

### 참조 규칙

- 이 문서를 수정하면 Claude Code에게 변경 사항을 알려주세요
- 구현 완료된 기능은 체크박스를 체크하세요 `[x]`
- 새로운 요구사항은 이 문서에 먼저 추가한 후 구현을 요청하세요

---

## 변경 이력

| 일시 | 버전 | 변경 내용 |
|------|------|-----------|
| YYYY-MM-DD | 0.1 | 초안 작성 |
