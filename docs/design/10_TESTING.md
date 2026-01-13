# 10. テスト設計（Feature Test中心）

## 方針
- 主要導線は Feature Test を優先
- UseCase単体テストは「複雑化した箇所のみ」追加
- DBはRefreshDatabase

## 最低限のFeature Test（MVP）
### 認証
- 登録できる
- ログイン/ログアウトできる
- パスワードリセット（メール送信はモック）

### 記事
- 非ログイン：published一覧/詳細が見られる、draftは404/403相当
- ログイン：作成できる（draft/published）
- 本人のみ編集/削除できる（他人は403）

### コメント
- ログイン：コメント投稿できる
- 本人のみ編集できる
- 本人または記事投稿者が削除できる

### お気に入り
- ログイン：お気に入り登録/解除できる
- 二重登録されない（unique）

### 検索
- qでヒットする
- tagでヒットする
- ページネーションが動く

## テストデータ（Seeder）
- User複数
- Post（draft/published混在）
- Tag付与
- Comment
- Favorite
