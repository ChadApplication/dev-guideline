# [Project Name] Development Specification (Brief)

> Date: YYYY-MM-DD | Base: `_template_latest`

---

## 1. Overview

**One-Line Summary**:

**Problem → Solution**:
-

**Tech Stack**: FastAPI + Next.js 15 + Prisma + Tailwind (default) / Additional:

---

## 2. Users and Roles

| Role | Description | Can See |
|------|-------------|---------|
| ADMIN | Administrator | Everything |
| USER | General user | Own data only |

---

## 3. Features

| # | Feature | Description | Priority |
|---|---------|-------------|----------|
| F01 | | | P0 |
| F02 | | | P0 |
| F03 | | | P1 |
| F04 | | | P2 |

---

## 4. Data

```prisma
model User {
  id       String @id @default(cuid())
  email    String? @unique
  password String?
  name     String?
  role     String @default("USER")
  // (add relations)
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

| Method | Path | Description | Auth |
|--------|------|-------------|------|
| GET | /api/health | Health check | X |
| POST | /api/{resource} | Create | O |
| GET | /api/{resource} | List | O |
| GET | /api/{resource}/{id} | Detail | O |
| PUT | /api/{resource}/{id} | Update | O |
| DELETE | /api/{resource}/{id} | Delete | O |

---

## 6. Screens

| Path | Screen | Key Elements |
|------|--------|-------------|
| /login | Login | Email + Password |
| /register | Register | Email + Password + Name |
| /dashboard | Dashboard | Card list, Create button, Role-based filter |
| /workspace/{id} | Main Feature | (description) |
| /settings | Settings | Profile, API keys |
| /admin | Admin | User list, Role management |

**Main Screen Wireframe**:
```
┌──────────────────────────────────┐
│ Header: Logo | [User] [Settings] │
├────────┬─────────────────────────┤
│ Side   │ Main Content            │
│ Step 1 │                         │
│ Step 2 │                         │
│ Step 3 │                         │
├────────┴─────────────────────────┤
│ Footer: [Previous] [Next]        │
└──────────────────────────────────┘
```

---

## 7. Implementation Order

```
Phase 0: Copy template, configure run.sh, venv + npm install
Phase 1: Apply Prisma schema, verify auth
Phase 2: Implement backend API (services/ + main.py)
Phase 3: Dashboard UI
Phase 4: Main feature UI
Phase 5: Error handling, dark mode, responsive finishing
```

---

## 8. Environment Variables

**backend/.env**: `PORT=8010` / API keys, etc.
**frontend/.env.local**: `NEXT_PUBLIC_BACKEND_PORT=8010` / `AUTH_SECRET=`

---

## Notes

-
