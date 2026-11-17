# ロードマップ

| フェーズ | 目安 | 内容 | ブランチ | 完了条件 |
|---|---|---|---|---|
| 0. 設計 | 2〜3日 | 要件定義、構成図、ER図、API設計、Git規約 | `feature/docs-design` | 設計書が develop にマージ済み |
| 1. 環境構築 | 1〜2日 | Docker、Laravel/React の雛形、Makefile | `feature/docker-env` ほか | `make init` で起動する |
| 2. 認証 | 2日 | 登録/ログイン/ログアウト/me | `feature/auth-api`, `feature/auth-ui` | ブラウザでログインできる |
| 3. プロジェクト | 2〜3日 | CRUD、メンバー、権限 | `feature/projects-api`, `feature/projects-ui` | 権限付きで共有できる |
| 4. タスク | 3〜4日 | CRUD、カンバン、絞り込み | `feature/tasks-api`, `feature/tasks-ui` | カンバンで運用できる |
| 5. コメント | 1〜2日 | タスクへのコメント | `feature/comments` | MVP 完成 |
| 6. 品質・CI | 2日 | 静的解析、Lint、GitHub Actions | `chore/ci-quality` | CI が全部通る |
| 7. リリース | 1日 | CHANGELOG、main へマージ、タグ付け | `release/1.0.0` | `v1.0.0` タグ |

## v1.1 以降の候補
- タスク割り当て時のメール通知（キュー）
- カンバンのドラッグ&ドロップ
- ファイル添付
- E2E テスト（Playwright）
