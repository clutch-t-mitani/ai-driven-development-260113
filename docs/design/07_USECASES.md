# 07. UseCase設計（業務処理一覧）

UseCase命名例：`app/UseCases/{Domain}/{Action}UseCase.php`

原則：
- 1UseCase = 1処理
- 入力：FormRequestから整形した配列/DTO
- 出力：必要最小限（Model/Collection/配列）

## Post

### Post/ListUseCase
- 概要：記事一覧取得（sort/tag/q）
- 入力：sort, tag, q, page
- 出力：Paginator（posts + withCount + tags + author）

備考：
- 非ログイン：publishedのみ
- ログイン：公開一覧はpublishedのみ（マイページでdraftは別）

### Post/ShowUseCase
- 概要：記事詳細取得
- 入力：post_id
- 出力：Post（tags, author, comments, counts）

備考：
- view_count increment（要件に応じて。MVPは単純incrementで可）

### Post/CreateUseCase
- 概要：記事作成
- 入力：user_id, title, body, status, tags[]
- 出力：Post

備考：
- tagsは slug生成し、tagsテーブルを upsert、post_tag同期

### Post/UpdateUseCase
- 概要：記事更新（本人のみ）
- 入力：post_id, title, body, status, tags[]
- 出力：Post

### Post/DeleteUseCase
- 概要：記事削除（soft delete、本人のみ）
- 入力：post_id
- 出力：void

## Comment

### Comment/CreateUseCase
- 概要：コメント作成
- 入力：post_id, user_id, body
- 出力：Comment

### Comment/UpdateUseCase
- 概要：コメント編集（本人のみ）
- 入力：comment_id, user_id, body
- 出力：Comment

### Comment/DeleteUseCase
- 概要：コメント削除（本人 or 記事投稿者）
- 入力：comment_id, actor_user_id
- 出力：void

## Favorite

### Favorite/ToggleCreateUseCase（または Create/Delete を分ける）
推奨：HTTPはPOST/DELETEで分け、UseCaseも分ける（シンプルで明確）

- Favorite/CreateUseCase：post_id, user_id -> favorite作成（unique制約で二重登録防止）
- Favorite/DeleteUseCase：post_id, user_id -> favorite削除

## Tag

### Tag/NormalizeUseCase（内部利用）
- 概要：入力文字列から tags[] を正規化し slug を生成
- 入力：raw tag string または配列
- 出力：[{name, slug}...]
