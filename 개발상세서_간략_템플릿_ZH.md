# [项目名] 开发详细说明书 (简略)

> 编写日期: YYYY-MM-DD | 基础: `_template_latest`

---

## 1. 概述

**一句话摘要**:

**问题 → 解决方案**:
-

**技术栈**: FastAPI + Next.js 15 + Prisma + Tailwind (默认) / 追加:

---

## 2. 用户及角色

| 角色 | 说明 | 可见范围 |
|------|------|----------|
| ADMIN | 管理员 | 全部 |
| USER | 普通用户 | 仅自己的数据 |

---

## 3. 功能

| # | 功能 | 说明 | 优先级 |
|---|------|------|--------|
| F01 | | | P0 |
| F02 | | | P0 |
| F03 | | | P1 |
| F04 | | | P2 |

---

## 4. 数据

```prisma
model User {
  id       String @id @default(cuid())
  email    String? @unique
  password String?
  name     String?
  role     String @default("USER")
  // (添加关系)
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

| Method | Path | 说明 | 认证 |
|--------|------|------|------|
| GET | /api/health | 状态检查 | X |
| POST | /api/{resource} | 创建 | O |
| GET | /api/{resource} | 列表 | O |
| GET | /api/{resource}/{id} | 详情 | O |
| PUT | /api/{resource}/{id} | 修改 | O |
| DELETE | /api/{resource}/{id} | 删除 | O |

---

## 6. 页面

| 路径 | 页面 | 核心元素 |
|------|------|----------|
| /login | 登录 | 邮箱 + 密码 |
| /register | 注册 | 邮箱 + 密码 + 姓名 |
| /dashboard | 仪表盘 | 卡片列表、创建按钮、按角色过滤 |
| /workspace/{id} | 主功能 | (说明) |
| /settings | 设置 | 个人资料、API 密钥 |
| /admin | 管理 | 用户列表、角色管理 |

**主页面线框图**:
```
┌──────────────────────────────────┐
│ Header: Logo | [用户] [设置]      │
├────────┬─────────────────────────┤
│ 侧边栏 │ 主要内容                │
│ Step 1 │                         │
│ Step 2 │                         │
│ Step 3 │                         │
├────────┴─────────────────────────┤
│ Footer: [上一步] [下一步]         │
└──────────────────────────────────┘
```

---

## 7. 实现顺序

```
Phase 0: 复制模板, 配置 run.sh, venv + npm install
Phase 1: 应用 Prisma 架构, 验证认证
Phase 2: 实现后端 API (services/ + main.py)
Phase 3: 仪表盘 UI
Phase 4: 主功能 UI
Phase 5: 错误处理、深色模式、响应式收尾
```

---

## 8. 环境变量

**backend/.env**: `PORT=8010` / API 密钥等
**frontend/.env.local**: `NEXT_PUBLIC_BACKEND_PORT=8010` / `AUTH_SECRET=`

---

## 备注

-
