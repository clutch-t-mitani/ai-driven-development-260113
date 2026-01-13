# 05. ルーティング/（将来）API設計

## Webルート（Blade前提）

### 公開（非ログイン可）
- GET /                 : トップ（記事一覧：新着）
- GET /posts            : 記事一覧（並び替え・タグ・検索クエリ対応）
- GET /posts/{post}     : 記事詳細（publishedのみ）
- GET /tags/{slug}      : タグ別一覧（/postsに統合でも可）
- GET /search           : 検索（/postsへ統合でも可）

### 認証
- GET /register, POST /register
- GET /login, POST /login
- POST /logout
- パスワードリセット一式（Laravel標準）

### ログイン必須
- GET /mypage                   : マイページ（投稿/お気に入り）
- GET /posts/create             : 記事作成
- POST /posts                   : 記事保存
- GET /posts/{post}/edit        : 記事編集
- PUT/PATCH /posts/{post}       : 記事更新
- DELETE /posts/{post}          : 記事削除（soft delete）
- POST /posts/{post}/comments   : コメント作成
- PATCH /comments/{comment}     : コメント更新
- DELETE /comments/{comment}    : コメント削除
- POST /posts/{post}/favorite   : お気に入り登録
- DELETE /posts/{post}/favorite : お気に入り解除

## クエリ仕様（一覧/検索）
- /posts?sort=new
- /posts?sort=popular
- /posts?q=keyword
- /posts?tag=laravel
- 併用：/posts?sort=popular&q=...&tag=...

## 将来のJSON API方針（REST想定）
- URIはWebと揃える（/api/posts 等）
- レスポンスはResource（JsonResource）で統一
- 認証はSanctum想定（セッション/トークンは要件次第）
