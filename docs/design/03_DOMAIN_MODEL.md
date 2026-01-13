# 03. ドメイン/データモデル設計

## エンティティ
- User
- Post
- Comment
- Favorite
- Tag
- PostTag（中間）

## 関係
- User 1 - N Post
- Post 1 - N Comment
- User 1 - N Comment
- Post N - N Tag（post_tag）
- User N - N Post（favorites：Userがお気に入りしたPost）

## 状態（Post）
- status: published | draft
- deleted_at: 論理削除
- view_count: 閲覧数（集計の起点。将来拡張）

## 集計（表示用）
- favorites_count（withCount）
- comments_count（withCount）
- view_count（postsに保持）

## 権限（要点）
- Post編集/削除：投稿者本人のみ
- Comment編集：投稿者本人のみ
- Comment削除：コメント投稿者 または 記事投稿者
- Favorite：本人のみ操作可能（自分の登録/解除のみ）
