# 08. セキュリティ/認可設計

## 認証
- Laravel Fortify または標準Auth相当を採用
- パスワード：bcrypt
- パスワードリセット：メール送信（Laravel標準）

メール認証（オプション）：
- users.email_verified_at を利用
- 投稿/コメント等を「認証済みのみ」にするかは運用方針で決める（MVPでは必須にしない）

## 認可（Policy）
必須：PostPolicy, CommentPolicy

### PostPolicy
- update/delete：post.user_id == auth()->id()

### CommentPolicy
- update：comment.user_id == auth()->id()
- delete：comment.user_id == auth()->id() OR comment.post.user_id == auth()->id()

Controllerでは authorize() を必ず呼ぶ。

## CSRF / XSS / SQLi
- CSRF：Laravel標準
- XSS：Bladeエスケープ徹底、本文は基本エスケープ（装飾したい場合は別途方針が必要）
- SQLi：Eloquent、クエリビルダを使用

## 追加の考慮（MVP）
- レート制限：ログイン、コメント投稿に適用を検討
- Soft delete：削除済みの露出を防ぐ（一覧/詳細で除外）
