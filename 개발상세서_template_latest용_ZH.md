# [项目名] 开发详细说明书

> 编写日期: YYYY-MM-DD | 复制来源: `_template_latest`
> 端口: Backend 80XX / Frontend 30XX

---

## 1. 项目

**一句话摘要**:

**问题 → 解决方案**:

---

## 2. 需要从模板修改的内容

### run.sh

```
PROJECT_NAME="[项目名]"
DEFAULT_BACKEND_PORT=80XX
DEFAULT_FRONTEND_PORT=30XX
```

### backend/requirements.txt 追加包

```
(默认包含: fastapi, uvicorn, python-multipart, pydantic, python-dotenv)
```

### frontend 追加包

```bash
npm install (追加包)
```

### backend/.env

```env
# (项目特定的追加环境变量)
```

---

## 3. 角色 (保持默认4种 / 如有变更请注明)

模板默认: ADMIN > MANAGER > USER > GUEST

变更事项:
- (无 / 或描述角色添加、删除、权限变更)

---

## 4. 追加数据模型

模板默认提供: User, Output, UserActivity, Account, Session (NextAuth)

```prisma
// (仅描述追加模型)
// model Project {
//   id     String @id @default(cuid())
//   title  String
//   userId String
//   user   User   @relation(fields: [userId], references: [id], onDelete: Cascade)
// }
```

文件存储结构 (如适用):
```
temp_uploads/{user_id}/{session_id}/
  (描述文件结构)
```

---

## 5. 追加 API

模板默认提供: /api/health, /api/sessions (CRUD), /api/progress, /api/variables, /api/user-settings, /api/llm-providers, /api/admin/*

| Method | Path | 说明 | 备注 |
|--------|------|------|------|
| | | | |
| | | | |

---

## 6. 页面

模板默认提供: /login, /register, /dashboard, /settings, /admin

### 追加/修改页面

| 路径 | 说明 |
|------|------|
| /workspace/{id} | (主功能页面) |
| | |

### 主功能页面布局

```
┌──────────────────────────────────┐
│ Header                           │
├────────┬─────────────────────────┤
│ 侧边栏 │ 主要内容                │
│        │                         │
│        │                         │
├────────┴─────────────────────────┤
│ Footer                           │
└──────────────────────────────────┘
```

---

## 7. 功能

| # | 功能 | 说明 | 优先级 |
|---|------|------|--------|
| F01 | | | P0 |
| F02 | | | P0 |
| F03 | | | P1 |

---

## 8. 实现顺序

```
Phase 0: cp -r _template_latest [项目名] → 修改 run.sh → venv + npm install → 验证启动
Phase 1: 添加 Prisma 架构 → npx prisma db push
Phase 2: backend/services/ 业务逻辑 → main.py API 添加
Phase 3: frontend/src/app/workspace/ 主页面实现
Phase 4: 收尾 (错误处理、深色模式、响应式)
```

---

## 备注

-
