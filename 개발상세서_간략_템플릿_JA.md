# [プロジェクト名] 開発詳細書 (簡略)

> 作成日: YYYY-MM-DD | ベース: `_template_latest`

---

## 1. 概要

**一行要約**:

**課題 → 解決**:
-

**技術スタック**: FastAPI + Next.js 15 + Prisma + Tailwind (デフォルト) / 追加:

---

## 2. ユーザーとロール

| ロール | 説明 | 閲覧可能範囲 |
|--------|------|--------------|
| ADMIN | 管理者 | すべて |
| USER | 一般ユーザー | 自分のデータのみ |

---

## 3. 機能

| # | 機能 | 説明 | 優先順位 |
|---|------|------|----------|
| F01 | | | P0 |
| F02 | | | P0 |
| F03 | | | P1 |
| F04 | | | P2 |

---

## 4. データ

```prisma
model User {
  id       String @id @default(cuid())
  email    String? @unique
  password String?
  name     String?
  role     String @default("USER")
  // (リレーション追加)
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

| Method | Path | 説明 | 認証 |
|--------|------|------|------|
| GET | /api/health | 状態確認 | X |
| POST | /api/{resource} | 作成 | O |
| GET | /api/{resource} | 一覧 | O |
| GET | /api/{resource}/{id} | 詳細 | O |
| PUT | /api/{resource}/{id} | 更新 | O |
| DELETE | /api/{resource}/{id} | 削除 | O |

---

## 6. 画面

| パス | 画面 | 主要要素 |
|------|------|----------|
| /login | ログイン | メール + パスワード |
| /register | 会員登録 | メール + パスワード + 名前 |
| /dashboard | ダッシュボード | カード一覧、作成ボタン、ロール別フィルタ |
| /workspace/{id} | メイン機能 | (説明) |
| /settings | 設定 | プロフィール、API キー |
| /admin | 管理者 | ユーザー一覧、ロール管理 |

**メイン画面ワイヤーフレーム**:
```
┌──────────────────────────────────┐
│ Header: ロゴ | [ユーザー] [設定]   │
├────────┬─────────────────────────┤
│ サイド  │ メインコンテンツ         │
│ Step 1 │                         │
│ Step 2 │                         │
│ Step 3 │                         │
├────────┴─────────────────────────┤
│ Footer: [前へ] [次へ]             │
└──────────────────────────────────┘
```

---

## 7. 実装順序

```
Phase 0: テンプレートコピー, run.sh 設定, venv + npm install
Phase 1: Prisma スキーマ反映, 認証確認
Phase 2: バックエンド API 実装 (services/ + main.py)
Phase 3: ダッシュボード UI
Phase 4: メイン機能 UI
Phase 5: エラー処理、ダークモード、レスポンシブ仕上げ
```

---

## 8. 環境変数

**backend/.env**: `PORT=8010` / API キーなど
**frontend/.env.local**: `NEXT_PUBLIC_BACKEND_PORT=8010` / `AUTH_SECRET=`

---

## メモ

-
