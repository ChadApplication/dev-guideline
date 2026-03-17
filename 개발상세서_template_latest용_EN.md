# [Project Name] Development Specification

> Date: YYYY-MM-DD | Source copy: `_template_latest`
> Ports: Backend 80XX / Frontend 30XX

---

## 1. Project

**One-Line Summary**:

**Problem → Solution**:

---

## 2. What to Change from the Template

### run.sh

```
PROJECT_NAME="[project_name]"
DEFAULT_BACKEND_PORT=80XX
DEFAULT_FRONTEND_PORT=30XX
```

### backend/requirements.txt Additional Packages

```
(Included by default: fastapi, uvicorn, python-multipart, pydantic, python-dotenv)
```

### frontend Additional Packages

```bash
npm install (additional packages)
```

### backend/.env

```env
# (Project-specific additional environment variables)
```

---

## 3. Roles (Keep default 4 types / Specify if changed)

Template default: ADMIN > MANAGER > USER > GUEST

Changes:
- (None / or describe role additions, removals, permission changes)

---

## 4. Additional Data Models

Template default provided: User, Output, UserActivity, Account, Session (NextAuth)

```prisma
// (Describe only additional models)
// model Project {
//   id     String @id @default(cuid())
//   title  String
//   userId String
//   user   User   @relation(fields: [userId], references: [id], onDelete: Cascade)
// }
```

File storage structure (if applicable):
```
temp_uploads/{user_id}/{session_id}/
  (describe file structure)
```

---

## 5. Additional API

Template default provided: /api/health, /api/sessions (CRUD), /api/progress, /api/variables, /api/user-settings, /api/llm-providers, /api/admin/*

| Method | Path | Description | Notes |
|--------|------|-------------|-------|
| | | | |
| | | | |

---

## 6. Screens

Template default provided: /login, /register, /dashboard, /settings, /admin

### Additional / Modified Screens

| Path | Description |
|------|-------------|
| /workspace/{id} | (Main feature screen) |
| | |

### Main Feature Screen Layout

```
┌──────────────────────────────────┐
│ Header                           │
├────────┬─────────────────────────┤
│ Side   │ Main Content            │
│        │                         │
│        │                         │
├────────┴─────────────────────────┤
│ Footer                           │
└──────────────────────────────────┘
```

---

## 7. Features

| # | Feature | Description | Priority |
|---|---------|-------------|----------|
| F01 | | | P0 |
| F02 | | | P0 |
| F03 | | | P1 |

---

## 8. Implementation Order

```
Phase 0: cp -r _template_latest [project_name] → edit run.sh → venv + npm install → verify startup
Phase 1: Add Prisma schema → npx prisma db push
Phase 2: backend/services/ business logic → main.py API additions
Phase 3: frontend/src/app/workspace/ main screen implementation
Phase 4: Finishing (error handling, dark mode, responsive)
```

---

## Notes

-
