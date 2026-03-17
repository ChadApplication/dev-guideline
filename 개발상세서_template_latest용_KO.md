# [프로젝트명] 개발상세서

> 작성일: YYYY-MM-DD | 복사원본: `_template_latest`
> 포트: Backend 80XX / Frontend 30XX

---

## 1. 프로젝트

**한 줄 요약**:

**문제 → 해결**:

---

## 2. 템플릿에서 변경할 것

### run.sh

```
PROJECT_NAME="[프로젝트명]"
DEFAULT_BACKEND_PORT=80XX
DEFAULT_FRONTEND_PORT=30XX
```

### backend/requirements.txt 추가 패키지

```
(기본 포함: fastapi, uvicorn, python-multipart, pydantic, python-dotenv)
```

### frontend 추가 패키지

```bash
npm install (추가 패키지)
```

### backend/.env

```env
# (프로젝트별 추가 환경변수)
```

---

## 3. 역할 (기본 4종 유지 / 변경 시 명시)

템플릿 기본: ADMIN > MANAGER > USER > GUEST

변경사항:
- (없음 / 또는 역할 추가·제거·권한 변경 기술)

---

## 4. 추가 데이터 모델

템플릿 기본 제공: User, Output, UserActivity, Account, Session (NextAuth)

```prisma
// (추가 모델만 기술)
// model Project {
//   id     String @id @default(cuid())
//   title  String
//   userId String
//   user   User   @relation(fields: [userId], references: [id], onDelete: Cascade)
// }
```

파일 저장 구조 (해당 시):
```
temp_uploads/{user_id}/{session_id}/
  (파일 구조 기술)
```

---

## 5. 추가 API

템플릿 기본 제공: /api/health, /api/sessions (CRUD), /api/progress, /api/variables, /api/user-settings, /api/llm-providers, /api/admin/*

| Method | Path | 설명 | 비고 |
|--------|------|------|------|
| | | | |
| | | | |

---

## 6. 화면

템플릿 기본 제공: /login, /register, /dashboard, /settings, /admin

### 추가·변경 화면

| 경로 | 설명 |
|------|------|
| /workspace/{id} | (메인 기능 화면) |
| | |

### 메인 기능 화면 레이아웃

```
┌──────────────────────────────────┐
│ Header                           │
├────────┬─────────────────────────┤
│ 사이드  │ 메인 콘텐츠             │
│        │                         │
│        │                         │
├────────┴─────────────────────────┤
│ Footer                           │
└──────────────────────────────────┘
```

---

## 7. 기능

| # | 기능 | 설명 | 우선순위 |
|---|------|------|----------|
| F01 | | | P0 |
| F02 | | | P0 |
| F03 | | | P1 |

---

## 8. 구현 순서

```
Phase 0: cp -r _template_latest [프로젝트명] → run.sh 수정 → venv + npm install → 기동 확인
Phase 1: Prisma 스키마 추가 → npx prisma db push
Phase 2: backend/services/ 비즈니스 로직 → main.py API 추가
Phase 3: frontend/src/app/workspace/ 메인 화면 구현
Phase 4: 마무리 (에러 처리, 다크 모드, 반응형)
```

---

## 메모

-
