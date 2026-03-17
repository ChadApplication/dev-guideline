# [プロジェクト名] 開発詳細書

> 作成日: YYYY-MM-DD | コピー元: `_template_latest`
> ポート: Backend 80XX / Frontend 30XX

---

## 1. プロジェクト

**一行要約**:

**課題 → 解決**:

---

## 2. テンプレートから変更するもの

### run.sh

```
PROJECT_NAME="[プロジェクト名]"
DEFAULT_BACKEND_PORT=80XX
DEFAULT_FRONTEND_PORT=30XX
```

### backend/requirements.txt 追加パッケージ

```
(デフォルト含む: fastapi, uvicorn, python-multipart, pydantic, python-dotenv)
```

### frontend 追加パッケージ

```bash
npm install (追加パッケージ)
```

### backend/.env

```env
# (プロジェクト別追加環境変数)
```

---

## 3. ロール (デフォルト4種を維持 / 変更時は明記)

テンプレートデフォルト: ADMIN > MANAGER > USER > GUEST

変更事項:
- (なし / またはロールの追加・削除・権限変更を記述)

---

## 4. 追加データモデル

テンプレートデフォルト提供: User, Output, UserActivity, Account, Session (NextAuth)

```prisma
// (追加モデルのみ記述)
// model Project {
//   id     String @id @default(cuid())
//   title  String
//   userId String
//   user   User   @relation(fields: [userId], references: [id], onDelete: Cascade)
// }
```

ファイル保存構造 (該当する場合):
```
temp_uploads/{user_id}/{session_id}/
  (ファイル構造を記述)
```

---

## 5. 追加 API

テンプレートデフォルト提供: /api/health, /api/sessions (CRUD), /api/progress, /api/variables, /api/user-settings, /api/llm-providers, /api/admin/*

| Method | Path | 説明 | 備考 |
|--------|------|------|------|
| | | | |
| | | | |

---

## 6. 画面

テンプレートデフォルト提供: /login, /register, /dashboard, /settings, /admin

### 追加・変更画面

| パス | 説明 |
|------|------|
| /workspace/{id} | (メイン機能画面) |
| | |

### メイン機能画面レイアウト

```
┌──────────────────────────────────┐
│ Header                           │
├────────┬─────────────────────────┤
│ サイド  │ メインコンテンツ         │
│        │                         │
│        │                         │
├────────┴─────────────────────────┤
│ Footer                           │
└──────────────────────────────────┘
```

---

## 7. 機能

| # | 機能 | 説明 | 優先順位 |
|---|------|------|----------|
| F01 | | | P0 |
| F02 | | | P0 |
| F03 | | | P1 |

---

## 8. 実装順序

```
Phase 0: cp -r _template_latest [プロジェクト名] → run.sh 修正 → venv + npm install → 起動確認
Phase 1: Prisma スキーマ追加 → npx prisma db push
Phase 2: backend/services/ ビジネスロジック → main.py API 追加
Phase 3: frontend/src/app/workspace/ メイン画面実装
Phase 4: 仕上げ (エラー処理、ダークモード、レスポンシブ)
```

---

## メモ

-
