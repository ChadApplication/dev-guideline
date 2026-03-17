# [项目名] 开发详细说明书

> 编写日期: YYYY-MM-DD
> 版本: 0.1 (草案)
> 基础模板: `_template_latest`
> 本文档为与 Claude Code 协作而优化的结构。

---

## 0. 本文档使用方法

本文档是向 Claude Code 说明项目并委托实现的**唯一事实来源**。

**编写顺序**: 从第1章开始按顺序填写。每一章都是下一章的输入。
**Claude Code 使用方法**: 将完成的章节传递给 Claude Code，即可实现相应部分。

```
1. 项目概述  →  做什么，为什么做
2. 用户定义  →  谁来使用
3. 功能列表  →  能做什么
4. 数据模型  →  需要什么数据
5. API 设计  →  前后端接口约定
6. 页面设计  →  用户看到什么
7. 实现步骤  →  按什么顺序构建
```

---

## 1. 项目概述

### 1.1 一句话摘要

> [用一句话描述这个应用是什么]

### 1.2 背景及目的

- **问题**: [当前存在什么问题]
- **解决方案**: [这个应用如何解决]
- **目标用户**: [谁来使用]

### 1.3 技术栈

| 层级 | 技术 | 备注 |
|------|------|------|
| Backend | FastAPI (Python 3.12) | 基于 `_template_latest` |
| Frontend | Next.js 15+ / React 19 / TypeScript | App Router |
| DB | SQLite (开发) → PostgreSQL (生产) | Prisma ORM |
| 认证 | NextAuth.js v5 | Credentials Provider |
| 样式 | Tailwind CSS | |
| 图标 | Lucide React | |
| AI/LLM | (使用时注明) | OpenAI / Anthropic / 本地 |

### 1.4 范围定义

**包含**:
- [ ] (本项目中必须实现的内容)
- [ ]
- [ ]

**排除** (未来考虑):
- (本版本不实现的内容)

---

## 2. 用户定义

### 2.1 角色

| 角色 | 说明 | 默认权限 |
|------|------|----------|
| ADMIN | 系统管理员 | 访问所有数据，管理用户 |
| USER | 普通用户 | 仅访问自己的数据 |
| (追加角色) | | |

### 2.2 用户场景

**场景 1**: [场景名称]
```
用户: [角色]
目标: [想要实现什么]
步骤:
  1. [操作]
  2. [操作]
  3. [操作]
结果: [预期结果]
```

**场景 2**: [场景名称]
```
用户: [角色]
目标:
步骤:
  1.
  2.
结果:
```

---

## 3. 功能列表

### 3.1 功能矩阵

为每个功能指定优先级。Claude Code 按 P0 开始的顺序实现。

| ID | 功能 | 说明 | 优先级 | 角色 |
|----|------|------|--------|------|
| F01 | | | P0 (必须) | |
| F02 | | | P0 (必须) | |
| F03 | | | P1 (重要) | |
| F04 | | | P2 (可选) | |

### 3.2 功能详情

每个功能的详细规格。Claude Code 阅读此部分进行实现。

#### F01: [功能名]

**说明**: [该功能做什么]

**输入**:
- [用户提供的内容]

**处理**:
- [系统执行的内容]

**输出**:
- [用户获得的结果]

**约束条件**:
- [大小限制、格式限制等]

**错误处理**:
- [无输入时] → [行为]
- [服务器错误时] → [行为]

---

#### F02: [功能名]

**说明**:

**输入**:
-

**处理**:
-

**输出**:
-

---

## 4. 数据模型

### 4.1 实体定义

以 Prisma 架构格式定义。Claude Code 直接将其应用于 `schema.prisma`。

```prisma
// === 用户 ===
model User {
  id        String   @id @default(cuid())
  username  String?  @unique
  email     String?  @unique
  password  String?
  name      String?
  role      String   @default("USER")
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  // 关系
  // projects  Project[]
}

// === (添加实体) ===
// model Project {
//   id        String   @id @default(cuid())
//   title     String
//   userId    String
//   user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)
//   createdAt DateTime @default(now())
//   updatedAt DateTime @updatedAt
// }
```

### 4.2 文件系统数据 (如适用)

如果后端以文件形式管理数据，请定义结构。

```
backend/temp_uploads/
  {user_id}/
    {session_id}/
      research_info.json      # 会话元数据
      processed_data.csv      # 处理后的数据
      {step}_results.json     # 各步骤结果
```

### 4.3 实体关系图 (文本)

```
User 1──N Project
Project 1──N Session
Session 1──N Result
```

---

## 5. API 设计

### 5.1 端点列表

Claude Code 阅读此表生成后端路由。

| Method | Path | 说明 | 认证 | 角色 |
|--------|------|------|------|------|
| GET | /api/health | 服务器状态检查 | X | 全部 |
| POST | /api/sessions | 创建新会话 | O | USER+ |
| GET | /api/sessions | 会话列表 | O | 按角色过滤 |
| DELETE | /api/sessions/{id} | 删除会话 | O | 所有者/ADMIN |
| | | | | |
| | | | | |

### 5.2 请求/响应详情

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

## 6. 页面设计

### 6.1 页面列表

| ID | 页面 | 路径 | 说明 | 认证 |
|----|------|------|------|------|
| S01 | 登录 | /login | 邮箱/密码登录 | X |
| S02 | 注册 | /register | 创建账户 | X |
| S03 | 仪表盘 | /dashboard | 项目列表、创建、管理 | O |
| S04 | (主功能) | /workspace/{id} | | O |
| S05 | 设置 | /settings | 用户设置，LLM 密钥管理 | O |
| S06 | 管理 | /admin | 用户管理 (仅 ADMIN) | O |

### 6.2 页面详情

#### S03: 仪表盘

**布局**:
```
┌─────────────────────────────────────────────────┐
│ Header: Logo | 项目名 | [用户菜单] [设置]         │
├─────────────────────────────────────────────────┤
│                                                 │
│  [+ 新建项目]                                    │
│                                                 │
│  ┌──────────────┐  ┌──────────────┐             │
│  │ 项目卡片      │  │ 项目卡片      │             │
│  │ 标题          │  │ 标题          │             │
│  │ 描述...       │  │ 描述...       │             │
│  │ 日期 | 删除   │  │ 日期 | 删除   │             │
│  └──────────────┘  └──────────────┘             │
│                                                 │
└─────────────────────────────────────────────────┘
```

**行为**:
- 点击项目卡片 → 跳转到 `/workspace/{id}`
- [+ 新建项目] → 标题输入弹窗 → 创建后跳转到 workspace
- 删除 → 确认对话框 → 删除后刷新列表

**按角色区分**:
- ADMIN: 显示所有用户的项目 (含所有者标签)
- MANAGER: 除 ADMIN 所有外的全部
- USER: 仅自己的项目
- GUEST: 仅标题/描述 (只读)

---

#### S04: [主功能页面]

**布局**:
```
┌─────────────────────────────────────────────────┐
│ Header: [← 列表] | 项目标题 | [设置]              │
├────────────┬────────────────────────────────────┤
│            │                                    │
│  侧边栏    │          主要内容                   │
│  (步骤     │                                    │
│   导航)    │                                    │
│            │                                    │
│  Step 1 ✓  │                                    │
│  Step 2 ●  │                                    │
│  Step 3    │                                    │
│            │                                    │
├────────────┴────────────────────────────────────┤
│ Footer: [上一步] [下一步] | 进度状态               │
└─────────────────────────────────────────────────┘
```

---

## 7. 实现步骤

按此顺序向 Claude Code 逐步下达指令。

### Phase 0: 项目搭建

```
指令: 复制 _template_latest 创建 {项目名}。
      修改 run.sh 中的 PROJECT_NAME 和端口。
      创建 backend venv，运行 frontend npm install。
```

**完成标准**: 通过 `./run.sh start` 双端服务器正常启动

### Phase 1: 数据模型 + 认证

```
指令: 将第4章的 Prisma 架构应用到 frontend/prisma/schema.prisma。
      使用 npx prisma db push 创建数据库。
      使用现有认证系统 (auth.ts, login, register 页面)。
```

**完成标准**: 注册 → 登录 → 可访问仪表盘

### Phase 2: 核心 API

```
指令: 在 backend/main.py 中实现第5章的 API 端点。
      在 services/ 目录中分离业务逻辑。
      每个端点可通过 curl 测试。
```

**完成标准**: 所有 API 正常响应 (curl 测试)

### Phase 3: 仪表盘 UI

```
指令: 实现第6章 S03 的仪表盘页面。
      基于现有 _template 的仪表盘进行修改。
      应用按角色过滤。
```

**完成标准**: 仪表盘上项目 CRUD 正常运行

### Phase 4: 主功能 UI

```
指令: 实现第6章 S04 的主功能页面。
      [添加功能特定指令]
```

**完成标准**: [功能特定标准]

### Phase 5: 收尾

```
指令: 添加错误处理、加载状态、空状态 UI。
      确认深色模式。
      确认响应式布局。
```

**完成标准**: 完整流程场景通过

---

## 8. 配置和环境变量

### backend/.env

```env
# 服务器
PORT=8010
HOST=0.0.0.0

# CORS
ALLOWED_ORIGINS=http://localhost:3010

# AI/LLM (如适用)
# OPENAI_API_KEY=
# ANTHROPIC_API_KEY=

# (项目特定添加)
```

### frontend/.env.local

```env
NEXT_PUBLIC_BACKEND_PORT=8010

# NextAuth
AUTH_SECRET=your-secret-key-here

# (项目特定添加)
```

---

## 9. Claude Code 协作技巧

### 指令模式

**功能实现请求**:
```
实现开发详细说明书中的 F01 功能。
- backend: 在 services/{service_name}.py 中编写逻辑
- backend: 在 main.py 中添加 API 端点
- frontend: 在 src/app/workspace/page.tsx 中实现 UI
```

**Bug 修复请求**:
```
在{页面}上发生{症状}。
预期行为: {正确行为}
当前行为: {错误行为}
```

**结构变更请求**:
```
已更新开发详细说明书第4章的数据模型。
请应用{变更内容}。
```

### 参考规则

- 修改本文档后请通知 Claude Code
- 已完成的功能请勾选 `[x]`
- 新需求请先添加到本文档，然后再请求实现

---

## 变更历史

| 日期 | 版本 | 变更内容 |
|------|------|----------|
| YYYY-MM-DD | 0.1 | 初稿编写 |
