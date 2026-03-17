# [プロジェクト名] 開発詳細書

> 作成日: YYYY-MM-DD
> バージョン: 0.1 (草案)
> ベーステンプレート: `_template_latest`
> このドキュメントは Claude Code との協業に最適化された構成です。

---

## 0. このドキュメントの使い方

このドキュメントは Claude Code にプロジェクトを説明し、実装を委任するための**唯一の真実のソース**です。

**記入順序**: 第1章から順番に埋めていきます。各章は次の章の入力となります。
**Claude Code の活用**: 完成したセクションを Claude Code に渡すと、該当部分を実装します。

```
1. プロジェクト概要  →  何を、なぜ作るか
2. ユーザー定義      →  誰が使うか
3. 機能一覧          →  何ができるか
4. データモデル      →  どんなデータが必要か
5. API 設計          →  フロント-バック間の契約
6. 画面設計          →  ユーザーが見るもの
7. 実装手順          →  どの順番で作るか
```

---

## 1. プロジェクト概要

### 1.1 一行要約

> [このアプリが何かを一文で]

### 1.2 背景と目的

- **課題**: [現在どんな問題があるか]
- **解決**: [このアプリがどう解決するか]
- **対象**: [誰が使うか]

### 1.3 技術スタック

| レイヤー | 技術 | 備考 |
|----------|------|------|
| Backend | FastAPI (Python 3.12) | `_template_latest` ベース |
| Frontend | Next.js 15+ / React 19 / TypeScript | App Router |
| DB | SQLite (開発) → PostgreSQL (本番) | Prisma ORM |
| 認証 | NextAuth.js v5 | Credentials Provider |
| スタイル | Tailwind CSS | |
| アイコン | Lucide React | |
| AI/LLM | (使用する場合は明記) | OpenAI / Anthropic / ローカル |

### 1.4 スコープ定義

**含む**:
- [ ] (このプロジェクトで必ず実装するもの)
- [ ]
- [ ]

**含まない** (将来の検討事項):
- (今回のバージョンでは実装しないもの)

---

## 2. ユーザー定義

### 2.1 ロール

| ロール | 説明 | デフォルト権限 |
|--------|------|----------------|
| ADMIN | システム管理者 | 全データアクセス、ユーザー管理 |
| USER | 一般ユーザー | 自分のデータのみアクセス |
| (追加ロール) | | |

### 2.2 ユーザーシナリオ

**シナリオ 1**: [シナリオ名]
```
ユーザー: [ロール]
目標: [達成したいこと]
手順:
  1. [アクション]
  2. [アクション]
  3. [アクション]
結果: [期待される結果]
```

**シナリオ 2**: [シナリオ名]
```
ユーザー: [ロール]
目標:
手順:
  1.
  2.
結果:
```

---

## 3. 機能一覧

### 3.1 機能マトリックス

各機能に優先順位を設定します。Claude Code は P0 から順に実装します。

| ID | 機能 | 説明 | 優先順位 | ロール |
|----|------|------|----------|--------|
| F01 | | | P0 (必須) | |
| F02 | | | P0 (必須) | |
| F03 | | | P1 (重要) | |
| F04 | | | P2 (任意) | |

### 3.2 機能詳細

各機能の詳細仕様です。Claude Code はこのセクションを読んで実装します。

#### F01: [機能名]

**説明**: [この機能が何をするか]

**入力**:
- [ユーザーが提供するもの]

**処理**:
- [システムが実行すること]

**出力**:
- [ユーザーが受け取る結果]

**制約条件**:
- [サイズ制限、フォーマット制限など]

**エラー処理**:
- [入力がない場合] → [動作]
- [サーバーエラー時] → [動作]

---

#### F02: [機能名]

**説明**:

**入力**:
-

**処理**:
-

**出力**:
-

---

## 4. データモデル

### 4.1 エンティティ定義

Prisma スキーマ形式で定義します。Claude Code はこれを直接 `schema.prisma` に反映します。

```prisma
// === ユーザー ===
model User {
  id        String   @id @default(cuid())
  username  String?  @unique
  email     String?  @unique
  password  String?
  name      String?
  role      String   @default("USER")
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  // リレーション
  // projects  Project[]
}

// === (エンティティ追加) ===
// model Project {
//   id        String   @id @default(cuid())
//   title     String
//   userId    String
//   user      User     @relation(fields: [userId], references: [id], onDelete: Cascade)
//   createdAt DateTime @default(now())
//   updatedAt DateTime @updatedAt
// }
```

### 4.2 ファイルシステムデータ (該当する場合)

バックエンドでファイルとして管理するデータがあれば、構造を定義します。

```
backend/temp_uploads/
  {user_id}/
    {session_id}/
      research_info.json      # セッションメタデータ
      processed_data.csv      # 処理済みデータ
      {step}_results.json     # 各ステップの結果
```

### 4.3 エンティティ関係図 (テキスト)

```
User 1──N Project
Project 1──N Session
Session 1──N Result
```

---

## 5. API 設計

### 5.1 エンドポイント一覧

Claude Code はこの表を読んでバックエンドルーターを生成します。

| Method | Path | 説明 | 認証 | ロール |
|--------|------|------|------|--------|
| GET | /api/health | サーバー状態確認 | X | 全体 |
| POST | /api/sessions | 新規セッション作成 | O | USER+ |
| GET | /api/sessions | セッション一覧 | O | ロール別フィルタ |
| DELETE | /api/sessions/{id} | セッション削除 | O | 所有者/ADMIN |
| | | | | |
| | | | | |

### 5.2 リクエスト/レスポンス詳細

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

## 6. 画面設計

### 6.1 画面一覧

| ID | 画面 | パス | 説明 | 認証 |
|----|------|------|------|------|
| S01 | ログイン | /login | メール/パスワードログイン | X |
| S02 | 会員登録 | /register | アカウント作成 | X |
| S03 | ダッシュボード | /dashboard | プロジェクト一覧、作成、管理 | O |
| S04 | (メイン機能) | /workspace/{id} | | O |
| S05 | 設定 | /settings | ユーザー設定、LLM キー管理 | O |
| S06 | 管理者 | /admin | ユーザー管理 (ADMIN のみ) | O |

### 6.2 画面詳細

#### S03: ダッシュボード

**レイアウト**:
```
┌─────────────────────────────────────────────────┐
│ Header: ロゴ | プロジェクト名 | [ユーザーメニュー] [設定] │
├─────────────────────────────────────────────────┤
│                                                 │
│  [+ 新規プロジェクト]                              │
│                                                 │
│  ┌──────────────┐  ┌──────────────┐             │
│  │ プロジェクトカード │ │ プロジェクトカード │           │
│  │ タイトル       │  │ タイトル       │             │
│  │ 説明...       │  │ 説明...       │             │
│  │ 日付 | 削除   │  │ 日付 | 削除   │             │
│  └──────────────┘  └──────────────┘             │
│                                                 │
└─────────────────────────────────────────────────┘
```

**動作**:
- プロジェクトカードクリック → `/workspace/{id}` に移動
- [+ 新規プロジェクト] → タイトル入力モーダル → 作成後 workspace に移動
- 削除 → 確認ダイアログ → 削除後リスト更新

**ロール別の違い**:
- ADMIN: 全ユーザーのプロジェクトを表示 (所有者ラベル付き)
- MANAGER: ADMIN 所有を除く全て
- USER: 自分のプロジェクトのみ
- GUEST: タイトル/説明のみ (読み取り専用)

---

#### S04: [メイン機能画面]

**レイアウト**:
```
┌─────────────────────────────────────────────────┐
│ Header: [← 一覧] | プロジェクトタイトル | [設定]    │
├────────────┬────────────────────────────────────┤
│            │                                    │
│  サイドバー │          メインコンテンツ            │
│  (ステップ  │                                    │
│   ナビ)    │                                    │
│            │                                    │
│  Step 1 ✓  │                                    │
│  Step 2 ●  │                                    │
│  Step 3    │                                    │
│            │                                    │
├────────────┴────────────────────────────────────┤
│ Footer: [前へ] [次へ] | 進捗状態                   │
└─────────────────────────────────────────────────┘
```

---

## 7. 実装手順

Claude Code にステップバイステップで指示する際は、この順序に従います。

### Phase 0: プロジェクトセットアップ

```
指示: _template_latest をコピーして {プロジェクト名} を作成。
      run.sh の PROJECT_NAME とポートを変更。
      backend venv 作成、frontend npm install。
```

**完了基準**: `./run.sh start` で両サーバーが正常起動

### Phase 1: データモデル + 認証

```
指示: 第4章の Prisma スキーマを frontend/prisma/schema.prisma に反映。
      npx prisma db push で DB 作成。
      既存の認証システムを活用 (auth.ts, login, register ページ)。
```

**完了基準**: 会員登録 → ログイン → ダッシュボードアクセス可能

### Phase 2: コア API

```
指示: 第5章の API エンドポイントを backend/main.py に実装。
      services/ ディレクトリにビジネスロジックを分離。
      各エンドポイントを curl でテスト可能に。
```

**完了基準**: 全 API が正常にレスポンス (curl テスト)

### Phase 3: ダッシュボード UI

```
指示: 第6章 S03 のダッシュボード画面を実装。
      既存の _template のダッシュボードをベースに修正。
      ロール別フィルタリングを適用。
```

**完了基準**: ダッシュボードでプロジェクト CRUD が動作

### Phase 4: メイン機能 UI

```
指示: 第6章 S04 のメイン機能画面を実装。
      [機能別の具体的な指示を追加]
```

**完了基準**: [機能別の完了基準]

### Phase 5: 仕上げ

```
指示: エラー処理、ローディング状態、空状態 UI を追加。
      ダークモードを確認。
      レスポンシブレイアウトを確認。
```

**完了基準**: 全フローシナリオが通過

---

## 8. 設定と環境変数

### backend/.env

```env
# サーバー
PORT=8010
HOST=0.0.0.0

# CORS
ALLOWED_ORIGINS=http://localhost:3010

# AI/LLM (該当する場合)
# OPENAI_API_KEY=
# ANTHROPIC_API_KEY=

# (プロジェクト別追加)
```

### frontend/.env.local

```env
NEXT_PUBLIC_BACKEND_PORT=8010

# NextAuth
AUTH_SECRET=your-secret-key-here

# (プロジェクト別追加)
```

---

## 9. Claude Code 協業のコツ

### 指示パターン

**機能実装リクエスト**:
```
開発詳細書の F01 機能を実装せよ。
- backend: services/{service_name}.py にロジックを記述
- backend: main.py に API エンドポイントを追加
- frontend: src/app/workspace/page.tsx に UI を実装
```

**バグ修正リクエスト**:
```
{画面}で{症状}が発生。
期待動作: {正しい動作}
現在の動作: {誤った動作}
```

**構造変更リクエスト**:
```
開発詳細書第4章のデータモデルを更新した。
{変更内容}を反映せよ。
```

### 参照ルール

- このドキュメントを修正したら Claude Code に変更を知らせてください
- 実装完了した機能はチェックボックスにチェック `[x]`
- 新しい要件はこのドキュメントに先に追加してから実装を依頼してください

---

## 変更履歴

| 日時 | バージョン | 変更内容 |
|------|-----------|----------|
| YYYY-MM-DD | 0.1 | 初稿作成 |
