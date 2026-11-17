# API 設計

- ベースパス: `/api`
- 形式: JSON（リクエストヘッダー `Accept: application/json`）
- 認証: Sanctum のセッション Cookie（最初に `GET /sanctum/csrf-cookie`）

## エンドポイント一覧

| メソッド | パス | 説明 | 成功 | 認証 |
|---|---|---|---|---|
| GET | `/sanctum/csrf-cookie` | CSRF Cookie 取得 | 204 | 不要 |
| POST | `/api/register` | 登録してログイン | 201 | 不要 |
| POST | `/api/login` | ログイン | 200 | 不要 |
| POST | `/api/logout` | ログアウト | 204 | 必要 |
| GET | `/api/me` | ログイン中のユーザー | 200 | 必要 |
| GET | `/api/projects` | 所属プロジェクト一覧（`?page=`） | 200 | 必要 |
| POST | `/api/projects` | 作成 | 201 | 必要 |
| GET | `/api/projects/{id}` | 詳細（メンバー含む） | 200 | 必要 |
| PATCH | `/api/projects/{id}` | 更新 | 200 | 必要 |
| DELETE | `/api/projects/{id}` | 削除 | 204 | 必要 |
| GET | `/api/projects/{id}/members` | メンバー一覧 | 200 | 必要 |
| POST | `/api/projects/{id}/members` | 追加 `{email, role}` | 201 | 必要 |
| DELETE | `/api/projects/{id}/members/{userId}` | 削除 | 204 | 必要 |
| GET | `/api/projects/{id}/tasks` | タスク一覧（`?status=&assignee_id=&q=`） | 200 | 必要 |
| POST | `/api/projects/{id}/tasks` | 作成 | 201 | 必要 |
| GET | `/api/tasks/{id}` | 詳細 | 200 | 必要 |
| PATCH | `/api/tasks/{id}` | 更新（部分更新可） | 200 | 必要 |
| DELETE | `/api/tasks/{id}` | 削除 | 204 | 必要 |
| GET | `/api/tasks/{id}/comments` | コメント一覧 | 200 | 必要 |
| POST | `/api/tasks/{id}/comments` | 投稿 | 201 | 必要 |
| DELETE | `/api/comments/{id}` | 削除 | 204 | 必要 |

## ステータスコード

| コード | 意味 | 例 |
|---|---|---|
| 200 / 201 / 204 | 成功 / 作成した / 成功（中身なし） | |
| 401 | ログインしていない | |
| 403 | 権限がない | member がプロジェクトを削除しようとした |
| 404 | 存在しない | |
| 419 | CSRF トークン切れ | |
| 422 | 入力エラー | タイトルが空 |
| 429 | リクエストが多すぎる | ログインを1分に6回試した |

## レスポンス例

### タスク（GET /api/tasks/12）
```json
{
  "id": 12,
  "project_id": 3,
  "title": "ログイン画面を作る",
  "description": null,
  "status": "in_progress",
  "priority": "high",
  "due_date": "2026-11-30",
  "position": 0,
  "assignee": { "id": 2, "name": "Hanako", "email": "hanako@example.com" },
  "creator": { "id": 1, "name": "Taro", "email": "taro@example.com" },
  "comments_count": 3,
  "created_at": "2026-11-17T10:00:00+09:00",
  "updated_at": "2026-11-17T10:00:00+09:00"
}
```

### 一覧（GET /api/projects）
```json
{
  "data": [ { "id": 3, "name": "TaskFlow 開発", "...": "..." } ],
  "links": { "first": "...", "last": "...", "prev": null, "next": null },
  "meta": { "current_page": 1, "last_page": 1, "per_page": 20, "total": 1 }
}
```

### エラー（422）
```json
{
  "message": "The title field is required.",
  "errors": { "title": ["The title field is required."] }
}
```
