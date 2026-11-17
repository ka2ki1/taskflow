# ADR-0001: 認証に Sanctum の SPA Cookie 認証を使う

- 日付: 2026-11-17
- ステータス: 採用

## 状況
React SPA から Laravel API を呼ぶ。認証方法として以下を検討した。
- A. API トークンを発行し、localStorage に保存して Authorization ヘッダーで送る
- B. Sanctum の SPA 認証（セッション Cookie + CSRF トークン）

## 決定
B を採用する。nginx で API と画面を同一オリジンにまとめる。

## 理由
- Cookie は HttpOnly なので JavaScript から読めず、XSS でトークンを盗まれない
- CSRF は Sanctum の XSRF-TOKEN で防げる
- Laravel 標準の仕組みなので実装量が少ない

## 影響
- モバイルアプリに対応するときは API トークン方式を別途追加する（その時に ADR を追加）
- 開発環境でもフロントは必ず nginx 経由（localhost:8080）でアクセスする
