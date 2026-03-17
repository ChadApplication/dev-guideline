# [Project Name] Development Specification

> Date: YYYY-MM-DD
> Version: 0.1 (Draft)
> Base Template: `_template_latest`
> This document is structured for optimal collaboration with Claude Code.

---

## 0. How to Use This Document

This document is the **single source of truth** for explaining and delegating implementation to Claude Code.

**Writing Order**: Fill in from Chapter 1 sequentially. Each chapter serves as input for the next.
**Using with Claude Code**: Pass completed sections to Claude Code for implementation.

```
1. Project Overview  →  What and why are we building
2. User Definition   →  Who uses it
3. Feature List      →  What can users do
4. Data Model        →  What data is needed
5. API Design        →  Frontend-backend contract
6. Screen Design     →  What users see
7. Implementation    →  In what order to build
```

---

## 1. Project Overview

### 1.1 One-Line Summary

> [Describe what this app does in one sentence]

### 1.2 Background and Purpose

- **Problem**: [What problem currently exists]
- **Solution**: [How this app solves it]
- **Target**: [Who will use it]

### 1.3 Tech Stack

| Layer | Technology | Notes |
|-------|-----------|-------|
| Backend | FastAPI (Python 3.12) | Based on `_template_latest` |
| Frontend | Next.js 15+ / React 19 / TypeScript | App Router |
| DB | SQLite (dev) → PostgreSQL (prod) | Prisma ORM |
| Auth | NextAuth.js v5 | Credentials Provider |
| Styling | Tailwind CSS | |
| Icons | Lucide React | |
| AI/LLM | (specify if used) | OpenAI / Anthropic / Local |

### 1.4 Scope Definition

**Included**:
- [ ] (Must implement in this project)
- [ ]
- [ ]

**Excluded** (future consideration):
- (Not implementing in this version)

---

## 2. User Definition

### 2.1 Roles

| Role | Description | Default Permissions |
|------|-------------|-------------------|
| ADMIN | System administrator | Access all data, manage users |
| USER | General user | Access own data only |
| (Additional roles) | | |

### 2.2 User Scenarios

**Scenario 1**: [Scenario name]
```
User: [Role]
Goal: [What they want to achieve]
Steps:
  1. [Action]
  2. [Action]
  3. [Action]
Result: [Expected outcome]
```

**Scenario 2**: [Scenario name]
```
User: [Role]
Goal:
Steps:
  1.
  2.
Result:
```

---

## 3. Feature List

### 3.1 Feature Matrix

Assign priority to each feature. Claude Code implements from P0 in order.

| ID | Feature | Description | Priority | Role |
|----|---------|-------------|----------|------|
| F01 | | | P0 (Required) | |
| F02 | | | P0 (Required) | |
| F03 | | | P1 (Important) | |
| F04 | | | P2 (Optional) | |

### 3.2 Feature Details

Detailed specifications for each feature. Claude Code reads this section for implementation.

#### F01: [Feature Name]

**Description**: [What this feature does]

**Input**:
- [What the user provides]

**Processing**:
- [What the system performs]

**Output**:
- [Result the user receives]

**Constraints**:
- [Size limits, format restrictions, etc.]

**Error Handling**:
- [When input is missing] → [Behavior]
- [On server error] → [Behavior]

---

#### F02: [Feature Name]

**Description**:

**Input**:
-

**Processing**:
-

**Output**:
-

---

## 4. Data Model

### 4.1 Entity Definition

Defined in Prisma schema format. Claude Code applies this directly to `schema.prisma`.

```prisma
// === Users ===
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

// === (Add entities) ===
// model Project {
//   id        String   @id @default(cuid())
//   title     String
//   userId    String
//   user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)
//   createdAt DateTime @default(now())
//   updatedAt DateTime @updatedAt
// }
```

### 4.2 File System Data (if applicable)

Define the structure if the backend manages data as files.

```
backend/temp_uploads/
  {user_id}/
    {session_id}/
      research_info.json      # Session metadata
      processed_data.csv      # Processed data
      {step}_results.json     # Results per step
```

### 4.3 Entity Relationship Diagram (Text)

```
User 1──N Project
Project 1──N Session
Session 1──N Result
```

---

## 5. API Design

### 5.1 Endpoint List

Claude Code reads this table to generate backend routers.

| Method | Path | Description | Auth | Role |
|--------|------|-------------|------|------|
| GET | /api/health | Server health check | X | All |
| POST | /api/sessions | Create new session | O | USER+ |
| GET | /api/sessions | List sessions | O | Filtered by role |
| DELETE | /api/sessions/{id} | Delete session | O | Owner/ADMIN |
| | | | | |
| | | | | |

### 5.2 Request/Response Details

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

## 6. Screen Design

### 6.1 Screen List

| ID | Screen | Path | Description | Auth |
|----|--------|------|-------------|------|
| S01 | Login | /login | Email/password login | X |
| S02 | Register | /register | Account creation | X |
| S03 | Dashboard | /dashboard | Project list, create, manage | O |
| S04 | (Main Feature) | /workspace/{id} | | O |
| S05 | Settings | /settings | User settings, LLM key management | O |
| S06 | Admin | /admin | User management (ADMIN only) | O |

### 6.2 Screen Details

#### S03: Dashboard

**Layout**:
```
┌─────────────────────────────────────────────────┐
│ Header: Logo | Project Name | [User Menu] [Settings] │
├─────────────────────────────────────────────────┤
│                                                 │
│  [+ New Project]                                │
│                                                 │
│  ┌──────────────┐  ┌──────────────┐             │
│  │ Project Card  │  │ Project Card  │             │
│  │ Title         │  │ Title         │             │
│  │ Desc...       │  │ Desc...       │             │
│  │ Date | Delete │  │ Date | Delete │             │
│  └──────────────┘  └──────────────┘             │
│                                                 │
└─────────────────────────────────────────────────┘
```

**Behavior**:
- Click project card → Navigate to `/workspace/{id}`
- [+ New Project] → Title input modal → Navigate to workspace after creation
- Delete → Confirmation dialog → Refresh list after deletion

**Role-based Differences**:
- ADMIN: Shows all users' projects (with owner label)
- MANAGER: All except ADMIN-owned
- USER: Own projects only
- GUEST: Title/description only (read-only)

---

#### S04: [Main Feature Screen]

**Layout**:
```
┌─────────────────────────────────────────────────┐
│ Header: [← List] | Project Title | [Settings]   │
├────────────┬────────────────────────────────────┤
│            │                                    │
│  Sidebar   │          Main Content              │
│  (Step     │                                    │
│   Nav)     │                                    │
│            │                                    │
│  Step 1 ✓  │                                    │
│  Step 2 ●  │                                    │
│  Step 3    │                                    │
│            │                                    │
├────────────┴────────────────────────────────────┤
│ Footer: [Previous] [Next] | Progress Status     │
└─────────────────────────────────────────────────┘
```

---

## 7. Implementation Steps

Follow this order when giving step-by-step instructions to Claude Code.

### Phase 0: Project Setup

```
Instruction: Copy _template_latest to create {project_name}.
             Change PROJECT_NAME and port in run.sh.
             Create backend venv, run frontend npm install.
```

**Completion Criteria**: Both servers start successfully with `./run.sh start`

### Phase 1: Data Model + Auth

```
Instruction: Apply Prisma schema from Chapter 4 to frontend/prisma/schema.prisma.
             Create DB with npx prisma db push.
             Use existing auth system (auth.ts, login, register pages).
```

**Completion Criteria**: Register → Login → Dashboard access works

### Phase 2: Core API

```
Instruction: Implement API endpoints from Chapter 5 in backend/main.py.
             Separate business logic in services/ directory.
             Each endpoint testable via curl.
```

**Completion Criteria**: All APIs respond correctly (curl test)

### Phase 3: Dashboard UI

```
Instruction: Implement dashboard screen from Chapter 6 S03.
             Modify based on existing _template dashboard.
             Apply role-based filtering.
```

**Completion Criteria**: Project CRUD works on dashboard

### Phase 4: Main Feature UI

```
Instruction: Implement main feature screen from Chapter 6 S04.
             [Add feature-specific instructions]
```

**Completion Criteria**: [Feature-specific criteria]

### Phase 5: Finishing

```
Instruction: Add error handling, loading states, empty state UI.
             Verify dark mode.
             Verify responsive layout.
```

**Completion Criteria**: Full flow scenario passes

---

## 8. Configuration and Environment Variables

### backend/.env

```env
# Server
PORT=8010
HOST=0.0.0.0

# CORS
ALLOWED_ORIGINS=http://localhost:3010

# AI/LLM (if applicable)
# OPENAI_API_KEY=
# ANTHROPIC_API_KEY=

# (Project-specific additions)
```

### frontend/.env.local

```env
NEXT_PUBLIC_BACKEND_PORT=8010

# NextAuth
AUTH_SECRET=your-secret-key-here

# (Project-specific additions)
```

---

## 9. Claude Code Collaboration Tips

### Instruction Patterns

**Feature Implementation Request**:
```
Implement feature F01 from the dev spec.
- backend: Write logic in services/{service_name}.py
- backend: Add API endpoint to main.py
- frontend: Implement UI in src/app/workspace/page.tsx
```

**Bug Fix Request**:
```
{Symptom} occurs on {screen}.
Expected behavior: {correct behavior}
Current behavior: {incorrect behavior}
```

**Structure Change Request**:
```
Updated the data model in Chapter 4 of the dev spec.
Apply {change description}.
```

### Reference Rules

- Notify Claude Code when you modify this document
- Check completed features with checkbox `[x]`
- Add new requirements to this document first, then request implementation

---

## Change History

| Date | Version | Changes |
|------|---------|---------|
| YYYY-MM-DD | 0.1 | Initial draft |
