# [프로젝트명] 개발상세서 (간략)

> 작성일: YYYY-MM-DD | 기반: `_template_latest`

---

## 1. 개요

**한 줄 요약**:

**문제 → 해결**:
-

**기술 스택**: FastAPI + Next.js 15 + Prisma + Tailwind (기본) / 추가:

---

## 2. 사용자 및 역할

| 역할 | 설명 | 볼 수 있는 것 |
|------|------|---------------|
| ADMIN | 관리자 | 전체 |
| USER | 일반 | 자기 것만 |

---

## 3. 기능

| # | 기능 | 설명 | 우선순위 |
|---|------|------|----------|
| F01 | | | P0 |
| F02 | | | P0 |
| F03 | | | P1 |
| F04 | | | P2 |

---

## 4. 데이터

```prisma
model User {
  id       String @id @default(cuid())
  email    String? @unique
  password String?
  name     String?
  role     String @default("USER")
  // (관계 추가)
}

// model Example {
//   id     String @id @default(cuid())
//   title  String
//   userId String
//   user   User   @relation(fields: [userId], references: [id], onDelete: Cascade)
// }
```

---

## 5. API

| Method | Path | 설명 | 인증 |
|--------|------|------|------|
| GET | /api/health | 상태 확인 | X |
| POST | /api/{resource} | 생성 | O |
| GET | /api/{resource} | 목록 | O |
| GET | /api/{resource}/{id} | 상세 | O |
| PUT | /api/{resource}/{id} | 수정 | O |
| DELETE | /api/{resource}/{id} | 삭제 | O |

---

## 6. 화면

| 경로 | 화면 | 핵심 요소 |
|------|------|-----------|
| /login | 로그인 | 이메일 + 비밀번호 |
| /register | 회원가입 | 이메일 + 비밀번호 + 이름 |
| /dashboard | 대시보드 | 카드 목록, 생성 버튼, 역할별 필터 |
| /workspace/{id} | 메인 기능 | (설명) |
| /settings | 설정 | 프로필, API 키 |
| /admin | 관리자 | 사용자 목록, 역할 관리 |

**메인 화면 와이어프레임**:
```
┌──────────────────────────────────┐
│ Header: 로고 | [사용자] [설정]    │
├────────┬─────────────────────────┤
│ 사이드  │ 메인 콘텐츠             │
│ Step 1 │                         │
│ Step 2 │                         │
│ Step 3 │                         │
├────────┴─────────────────────────┤
│ Footer: [이전] [다음]             │
└──────────────────────────────────┘
```

---

## 7. 구현 순서

```
Phase 0: 템플릿 복사, run.sh 설정, venv + npm install
Phase 1: Prisma 스키마 반영, 인증 확인
Phase 2: 백엔드 API 구현 (services/ + main.py)
Phase 3: 대시보드 UI
Phase 4: 메인 기능 UI
Phase 5: 에러 처리, 다크 모드, 반응형 마무리
```

---

## 8. 환경변수

**backend/.env**: `PORT=8010` / API 키 등
**frontend/.env.local**: `NEXT_PUBLIC_BACKEND_PORT=8010` / `AUTH_SECRET=`

---

## 메모

-
